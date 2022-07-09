# Guida di Stile Airbnb JavaScript Style Guide() {

*Un approccio per lo più ragionevole a JavaScript*

> **Note**: questa guida presuppone che si stia usando [Babel](https://babeljs.io), e richiede di usare [babel-preset-airbnb](https://npmjs.com/babel-preset-airbnb) o equivalente. Presuppone anche che si stia installando shims/polyfills nella propria app, con [airbnb-browser-shims](https://npmjs.com/airbnb-browser-shims) o equivalente.

[![Downloads](https://img.shields.io/npm/dm/eslint-config-airbnb.svg)](https://www.npmjs.com/package/eslint-config-airbnb)
[![Downloads](https://img.shields.io/npm/dm/eslint-config-airbnb-base.svg)](https://www.npmjs.com/package/eslint-config-airbnb-base)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

Questa guida è disponibile anche in altre lingue. Vedere [Traduzioni](#traduzioni)

Altre Guide di Stile

  - [ES5 (Deprecated)](https://github.com/airbnb/javascript/tree/es5-deprecated/es5)
  - [React](react/)
  - [CSS-in-JavaScript](css-in-javascript/)
  - [CSS & Sass](https://github.com/airbnb/css)
  - [Ruby](https://github.com/airbnb/ruby)

## Sommario

  1. [Tipi](#Tipi)
  1. [Riferimenti](#Riferimenti)
  1. [Oggetti](#Oggetti)
  1. [Array](#Array)
  1. [Destrutturazione](#Destrutturazione)
  1. [Stringhe](#Stringhe)
  1. [Funzioni](#Funzioni)
  1. [Funzioni Arrow](#arrow-functions)
  1. [Classi e Costruttori](#classei--costructtori)
  1. [Moduli](#Moduli)
  1. [Iteratori e Generatori](#iteratori-e-generatori)
  1. [Proprietà](#Proprietà)
  1. [Variabili](#Variabili)
  1. [Hoisting](#hoisting)
  1. [Operatori di Confronto e Uguaglianza](#operatori-confronto--uguaglianza)
  1. [Blocchi](#Blocchi)
  1. [Istruzioni di Controllo](#istruzioni-di-controllo)
  1. [Commenti](#commenti)
  1. [Spazi](#Spazi)
  1. [Virgole](#Virgole)
  1. [Punti e Virgola](#Punti--Virgola)
  1. [Cast di Tipi e Coercizioni](#cast-di-tipi--coercizioni)
  1. [Convenzioni di Denominazione](#convenzioni-di-denominazione)
  1. [Accessori](#accessori)
  1. [Eventi](#eventi)
  1. [jQuery](#jquery)
  1. [Compatibilità ECMAScript 5](#compatibilita-ecmascript-5)
  1. [Stili ECMAScript 6+ (ES 2015+)](#stili-ecmascript-6-es-2015)
  1. [Libreria Standard Library](#libreria-standard)
  1. [Test](#test)
  1. [Prestazioni](#Prestazioni)
  1. [Risorse](#Risorse)
  1. [Nella natura selvaggia](#nella-natura-selvaggia)
  1. [Traduzioni](#Traduzioni)
  1. [La Guida Guida di Stile JavaScript](#la-guida-guida-di-stile-javascript)
  1. [Chiacchiera con noi su JavaScript](#chiacchera-con-noi-su-javascript)
  1. [Contributori](#contributori)
  1. [Licenza](#Licenza)
  1. [Modifiche](#modifiche)

## Tipi

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **Primitivi**: Quando si accede a un tipo primitivo si lavora direttamente con il suo valore.

    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`
    - `symbol`
    - `bigint`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

    - Symbol e BigInts non possono essere fedelmente oggetto di polyfill, Quindi non dovrebbero essere usati quando si indirizzano browser/ambienti che non li supportano nativamente.

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex)  **Complessi**: Quando si accede a un tipo complesso si lavora su un riferimento al suo valore.

    - `object`
    - `array`
    - `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ torna all'inizio](#sommario)**

## Assegnazioni

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) Usare `const` per tutti le proprie assegnazioni; evitare di usare `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign)

    > Perche? Assicura che non si riassegnino le proprie assegnazioni, il che può portare a bug e difficoltà di comprensione del codice.

    ```javascript
    // male
    var a = 1;
    var b = 2;

    // bene
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>
  - [2.2](#references--disallow-var) Se si devono riassegnare assegnazioni, usare `let` invece di `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var)

    > Perche? La visibilità di `let` è a livello di blocco diversamente da `var` che è a livello di funzione.

    ```javascript
    // male
    var count = 1;
    if (true) {
      count += 1;
    }

    // bene, usare let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>
  - [2.3](#references--block-scope) Si noti che sia `let` che `const` hanno visibilità a livello di blocco, laddove  `var` ha visibilità a livello di funzione.

    ```javascript
    // const e let esistono solo nei blocchi nei quali sono definite.
    {
      let a = 1;
      const b = 1;
      var c = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    console.log(c); // Stampa 1
    ```

    Nel codice sopra, si può vedere che referenziare  `a` e `b` produrrà un  ReferenceError, mentre `c` contiene il numero. Questo perchè `a` e `b` sono visibili a livello di blocco mentre `c` è visibile alla funzione che la contiene.

**[⬆ torna all'inizio](#sommario)**

## Oggetti

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new) Usare la sintassi letterale per la creazione di oggetti. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object)

    ```javascript
    // male
    const item = new Object();

    // bene
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.4"></a>
  - [3.2](#es6-computed-properties) Usare nomi di proprietà calcolati nella creazione di oggetti con nomi di proprietà dinamici.

    > Perche? Consente di definire tutte le proprietà di un oggetto nello stesso posto.

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // male
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // bene
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.5"></a>
  - [3.3](#es6-object-shorthand) Usare la sintassi abbreviata per un metodo di oggetto. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)

    ```javascript
    // male
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // bene
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.6"></a>
  - [3.4](#es6-object-concise) Usare la sintassi abbreviata per i valori di proprietà. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)

    > Perche? È più concisa e descrittiva.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // male
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // bene
    const obj = {
      lukeSkywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.7"></a>
  - [3.5](#objects--grouped-shorthand) Raggruppare le proprietà con sintassi abbreviata all'inizio della dichiarazione del proprio oggetto.

    > Perche? È più facile dire quali proprietà utilizzano la sintassi abbreviata.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // male
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // bene
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.8"></a>
  - [3.6](#objects--quoted-props) Racchiudere tra apici solo le proprietà che hanno identificatori non validi. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props)

    > Perche? In generale le consideriamo oggettivamente più leggibili. Migliora l'evidenziazione della sintassi e è anche maggiormente ottimizzata per molti motori JS.

    ```javascript
    // male
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // bene
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--prototype-builtins"></a>
  - [3.7](#objects--prototype-builtins) Non chiamare metodi `Object.prototype` direttamente, tipo `hasOwnProperty`, `propertyIsEnumerable`, e `isPrototypeOf`. eslint: [`no-prototype-builtins`](https://eslint.org/docs/rules/no-prototype-builtins)

    > Perche? Questi metodi potrebbero essere oscurati da proprietà nell'oggetto in questione - si consideri `{ hasOwnProperty: false }` - oppure l'oggetto potrebbe essere un oggetto nullo (`Object.create(null)`).

    ```javascript
    // male
    console.log(object.hasOwnProperty(key));

    // bene
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // best
    const has = Object.prototype.hasOwnProperty; // cache la ricerca una volta sola, a livello di modulo.
    console.log(has.call(object, key));
    /* or */
    import has from 'has'; // https://www.npmjs.com/package/has
    console.log(has(object, key));
    ```

  <a name="objects--rest-spread"></a>
  - [3.8](#objects--rest-spread) Preferire la sintassi spread dell'oggetto al posto di [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) per copie per indirizzo (shallow) di oggetti. Usare la sintassi rest per i parametri per ottenere un nuovo oggetto con certe proprietà omesse.. eslint: [`prefer-object-spread`](https://eslint.org/docs/rules/prefer-object-spread)

    ```javascript
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // questo cambia `original` ಠ_ಠ
    delete copy.a; // so does this

    // male
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copia => { a: 1, b: 2, c: 3 }

    // bene
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copia => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ torna all'inizio](#sommario)**

## Array

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) Usare la sintassi letterale per la creazione di array. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor)

    ```javascript
    // male
    const items = new Array();

    // bene
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) Usare [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) invece dell'assegnazione diretta per aggiungere elementi a un array.

    ```javascript
    const someStack = [];

    // male
    someStack[someStack.length] = 'abracadabra';

    // bene
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads) Usare la sintassi spreads `...` per la copia di array.

    ```javascript
    // male
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // bene
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a>
  <a name="arrays--from-iterable"></a><a name="4.4"></a>
  - [4.4](#arrays--from-iterable) Per convertire un oggetto iterabile in un array usare la sintassi spread `...` invece di [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

    ```javascript
    const foo = document.querySelectorAll('.foo');

    // bene
    const nodes = Array.from(foo);

    // best
    const nodes = [...foo];
    ```

  <a name="arrays--from-array-like"></a>
  - [4.5](#arrays--from-array-like) Usare [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) per convertire un oggetto tipo array in un array.

    ```javascript
    const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

    // male
    const arr = Array.prototype.slice.call(arrLike);

    // bene
    const arr = Array.from(arrLike);
    ```

  <a name="arrays--mapping"></a>
  - [4.6](#arrays--mapping) Usare [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) instead of spread `...` for mapping over iterables, becaUsare it avoids creating an intermediate array.

    ```javascript
    // male
    const baz = [...foo].map(bar);

    // bene
    const baz = Array.from(foo, bar);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.7](#arrays--callback-return) Usare istruzioni return in metodi callback di array. Va bene omettere return se il corpo della funzione consiste in una singola istruzione che ritorna una espressione senza effetti collaterali, seguendo [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // bene
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // bene
    [1, 2, 3].map((x) => x + 1);

    // male - nessun valore ritornato significa che `acc` diventa indefinito
    // dopo la prima iterazione
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
    });

    // bene
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      return flatten;
    });

    // male
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // bene
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a>
  - [4.8](#arrays--bracket-newline) Usare interruzioni di riga dopo l'apertura e prima della chiusura delle parentesi di un array. Se l'array consta di righe multiple

    ```javascript
    // male
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    const numberInArray = [
      1, 2,
    ];

    // bene
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```

**[⬆ torna all'inizio](#sommario)**

## Destrutturazione

  <a name="destructuring--object"></a><a name="5.1"></a>
  - [5.1](#destructuring--object) Usare le destrutturazione di oggetto quando si accede e si usano multiple proprietà di un oggetto. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    > Perche? La destrutturazione evita al programmatore di creare assegnazioni temporanee per queste proprietà, e da accessi ripetuti all'oggetto. Accessi ripetuti all'oggetto creano più codice, richiedono ulteriore lettura, e creano opportunità per errori. La destrutturazione degli oggetti fornisce anche un singolo posto per la definizione della struttura dell'oggetto che è usato in quel blocco, in luogo di richiedere la lettura dell'intero blocco per determinare cosa si usi.

    ```javascript
    // male
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // bene
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  - [5.2](#destructuring--array) Usare la destrutturazione di array. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // male
    const first = arr[0];
    const second = arr[1];

    // bene
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>
  - [5.3](#destructuring--object-over-array) Usare la destrutturazione di oggetto per valori multipli da ritornare, non la destrutturazione di array.

    > Perche? Col tempo si possono aggiungere nuove proprietà o modificare l'ordine delle cose senza rompere i punti di chiamata<.

    ```javascript
    // male
    function processInput(input) {
      // poi è successo un miracolo
      return [left, right, top, bottom];
    }

    // il chiamante deve pensare all'ordine nel quale i dati vengono ritornati
    const [left, __, top] = processInput(input);

    // bene
    function processInput(input) {
      // poi è successo un miracolo
      return { left, right, top, bottom };
    }

    // il chiamante seleziona solo i dati che gli servono
    const { left, top } = processInput(input);
    ```

**[⬆ torna all'inizio](#sommario)**

## Stringhe

  <a name="strings--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes) Usare apici singoli `''` per le stringhe. eslint: [`quotes`](https://eslint.org/docs/rules/quotes)

    ```javascript
    // male
    const name = "Capt. Janeway";

    // male - template letterali dovrebbero contenere interpolazione o nuove righe
    const name = `Capt. Janeway`;

    // bene
    const name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) Stringhe che rendono la riga più lunga di 100 caratteri non dovrebbero essere scritte su righe diverse usando la concatenazione di stringa.

    > Perche? Stringhe spezzato sono difficili da utilizzare e rendono il codice meno ricercabile

    ```javascript
    // male
    const errorMessage = 'This is a super long error that was thrown becaUsare \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // male
    const errorMessage = 'This is a super long error that was thrown becaUsare ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // bene
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.4"></a>
  - [6.3](#es6-template-literals) Quando si costruiscono stringhe programmaticamente, usare i template di stringa invece della concatenazione. eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > Perche? I template di stringa forniscono una sintassi leggibile e concisa, con le appropriate funzionalità di ritorni a capo e interpolazione di stringa.

    ```javascript
    // male
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // male
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // male
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // bene
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

  <a name="strings--eval"></a><a name="6.5"></a>
  - [6.4](#strings--eval) Mai usare `eval()` su una stringa, apre troppo vulnerabilità. eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

  <a name="strings--escaping"></a>
  - [6.5](#strings--escaping) Non utilizzare sequenze di escape non necessarie nelle stringhe. eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

    > Perche? La barre rovesciate peggiorano la leggibilità, pertanto dovrebber essere presenti sono dove necessario.

    ```javascript
    // male
    const foo = '\'this\' \i\s \"quoted\"';

    // bene
    const foo = '\'this\' is "quoted"';
    const foo = `my name is '${name}'`;
    ```

**[⬆ torna all'inizio](#sommario)**

## Funzioni

  <a name="functions--declarations"></a><a name="7.1"></a>
  - [7.1](#functions--declarations) Usare espressioni di funzioni denominate invece di dichiarazione di funzioni+. eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

    > Perche? Le dichiarazioni di funzione sono portate all'inizio del blocco di utilizzo (*hoisted*), il che significa che è facie - troppo facile - fare riferimento alla funzione prima che essa sia definita nel file. Questo inficia la leggibilità e la manutenibilità. Se si ritiene che la definizione di una funzione sia grande o sufficientemente complessa da interferire con la comprensione del resto del file, allora forse è il momento di estrarla nel suo proprio modulo! Non si dimentichi di denominare esplicitamente l'espressione a prescindere dal fatto che il nome sia inferito dalla variabile contenitore (il che è spesso il caso nei browser moderni che usano compilatori tipo Babel). Questo elimina qualunque assunzione fatto circa il call stack di Error. ([Discussione](https://github.com/airbnb/javascript/issues/794))

    ```javascript
    // male
    function foo() {
      // ...
    }

    // male
    const foo = function () {
      // ...
    };

    // bene
    // nome lessicale distinto dall'invocazione della variabile referenziata
    const short = function longUniqueMoreDescriptiveLexicalFoo() {
      // ...
    };
    ```

  <a name="functions--iife"></a><a name="7.2"></a>
  - [7.2](#functions--iife) Racchiudere le espressioni di funzioni invocate immediatamente (IIFE) tra parentesi. eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife)

    > Perche? Una espressione di funzione invocata immediatamente è una singola unità, che racchiude se stessa e le sue parentesi di invocazione, tra parentesi. Si noti che in un modo con moduli dappertutto non sarà quasi mai necessaria una IIFE.

    ```javascript
    // immediately-invoked function expression (IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="7.3"></a>
  - [7.3](#functions--in-blocks) Mai dichiarare una funzione in un blocco di non funzione (`if`, `while`, ecc). Assegnare invece la funzione a una variabile. I browser lo consentono, ma l'interpretazione è per tutti diversa, il che è foriero di cattive notizie. eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func)

  <a name="functions--note-on-blocks"></a><a name="7.4"></a>
  - [7.4](#functions--note-on-blocks) **Note:** ECMA-262 definisce un blocco (`block`) come una lista di istruzioni. La dichiarazione di una funzione non è una istruzione.

    ```javascript
    // male
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // bene
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.5"></a>
  - [7.5](#functions--arguments-shadow) Mai denominare un parametro `arguments`. Questo otterrebbe la precedenza sull'oggetto `arguments` che viene passato od ogni blocco di visibilità di funzione.

    ```javascript
    // male
    function foo(name, options, arguments) {
      // ...
    }

    // bene
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="es6-rest"></a><a name="7.6"></a>
  - [7.6](#es6-rest) Mai usare `arguments`, preferire la sintassi rest `...`. eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

    > Perche? `...` è esplicita circa quali argomenti si vuole estrarre. Inoltre gli argomenti con la sintassi resto sono un vero Array, non un tipo Array come `arguments`.

    ```javascript
    // male
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // bene
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.7"></a>
  - [7.7](#es6-default-parameters) Usare la sintassi per i parametri predefiniti invece che argomenti di funzione mutabili.

    ```javascript
    // really bad
    function handleThings(opts) {
      // No! Non si dovrebbero cambiare gli argomenti di funzione
      // Doppio male: se opts è falsy sarebbe assegnato a un oggetto che potrebbe
      // essere quello che si vuole ma che può introdurre subdoli bug
      opts = opts || {};
      // ...
    }

    // comunque male
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // bene
    function handleThings(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.8"></a>
  - [7.8](#functions--default-side-effects) Evtiare effetti collaterali con i parametri predefiniti.

    > Perche? Sono confusi nel ragionarci.

    ```javascript
    var b = 1;
    // male
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.9"></a>
  - [7.9](#functions--defaults-last) Posizionare sempre in fondo i parametri predeefiniti. eslint: [`default-param-last`](https://eslint.org/docs/rules/default-param-last)

    ```javascript
    // male
    function handleThings(opts = {}, name) {
      // ...
    }

    // bene
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="7.10"></a>
  - [7.10](#functions--constructor) Mai usare il costruttore di funzione per creare una nuova funzione. eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

    > Perche? Creando una funzione in questo modo si elabora una stringa in modo simile a  `eval()`, il che apre vulnerabilità.

    ```javascript
    // male
    var add = new Function('a', 'b', 'return a + b');

    // still bad
    var subtract = Function('a', 'b', 'return a - b');
    ```

  <a name="functions--signature-spacing"></a><a name="7.11"></a>
  - [7.11](#functions--signature-spacing) Spaziature nella firma di una funzione. eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    > Perche? La consistenza è una buona cosa, e non si dovrebbero dover aggiungere o togliere uno spazio quando si aggiunge o toglie un nome.

    ```javascript
    // male
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // bene
    const x = function () {};
    const y = function a() {};
    ```

  <a name="functions--mutate-params"></a><a name="7.12"></a>
  - [7.12](#functions--mutate-params) Mai modificare parameters. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign)

    > Perche? La manipolazione di oggetti passati come parametri potrebbe causare effetti collaterali variabili non voluti nel chiamaante originale.

    ```javascript
    // male
    function f1(obj) {
      obj.key = 1;
    }

    // bene
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    }
    ```

  <a name="functions--reassign-params"></a><a name="7.13"></a>
  - [7.13](#functions--reassign-params) Mai riassegnare parametri. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign)

    > Perche? La riassegnazione di parametri può portare a comportamenti inaspettati, specialmente quando si accede all'oggetto `arguments`. Può anche causare problemi di ottimizzazione, specialmente in  V8.

    ```javascript
    // male
    function f1(a) {
      a = 1;
      // ...
    }

    function f2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // bene
    function f3(a) {
      const b = a || 1;
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.14"></a>
  - [7.14](#functions--spread-vs-apply) Preferire l'utilizzo della sintassi spread `...` to call funzioni variadiche. eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    > Perche? E' più pulito, non serve fornire un contesto, e non si può facilmente comporre `new` con `apply`.

    ```javascript
    // male
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // bene
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // male
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

    // bene
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"></a>
  - [7.15](#functions--signature-invocation-indentation) Funzioni con firma su più righe, o invocazioni, dovrebbero essere indentate proprio come qualsiasi altra lista su più righe in questa guida: con ciascun elemento su una riga a se stante, con una virgola in coda all'ultimo elemento. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

    ```javascript
    // male
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // bene
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // male
    console.log(foo,
      bar,
      baz);

    // bene
    console.log(
      foo,
      bar,
      baz,
    );
    ```

**[⬆ torna all'inizio](#sommario)**

## Funzioni Arrow 

  <a name="arrows--use-them"></a><a name="8.1"></a>
  - [8.1](#arrows--use-them) Quando si deve usare una funzione anonima (come per passare un callback in linea) usare la notazione di funzione arro. eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing)

    > Perche? Crea una versione della funzione che viene eseguita nel contesto di `this`, il che in genere è quallo che si vuole, e ha una sintassi più concisa.

    > Perchè no? Se si ha una funzione piuttosto complicata, si potrebbe spostare quella logica all'esterno nella sua propria espressione di funzione denominata.

    ```javascript
    // male
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // bene
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>
  - [8.2](#arrows--implicit-return) Se il corpo di una funzione ritorna una [espressione](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) senza effetti collaterali, omettere le parentesi graffe e usare il return implicito. Altrimenti mantenere le graffe e usare una istruzione `return`. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style)

    > Perche? Zucchero sintattico. C'è più leggibilità quando le funzioni sono concatenate.

    ```javascript
    // male
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // bene
    [1, 2, 3].map((number) => `A string containing the ${number + 1}.`);

    // bene
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // bene
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // Nessun return implicito con effetti collaterali
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Do something if callback returns true
      }
    }

    let bool = false;

    // male
    foo(() => bool = true);

    // bene
    foo(() => {
      bool = true;
    });
    ```

  <a name="arrows--paren-wrap"></a><a name="8.3"></a>
  - [8.3](#arrows--paren-wrap) Se l'espressione si trova su più righe, racchiuderla tra parentesi per una migliore leggibilità.

    > Perche? Mostra chiaramente dove inizia e finisce la funzione.

    ```javascript
    // male
    ['get', 'post', 'put'].map((httpMethod) => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // bene
    ['get', 'post', 'put'].map((httpMethod) => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```

  <a name="arrows--one-arg-parens"></a><a name="8.4"></a>
  - [8.4](#arrows--one-arg-parens) Racchiudere sempre tra parentesi gli argomenti per chiarezza e consistenza. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens)

    > Perche? Minimizza differenze quando si aggiungono o rimuovono gli argomenti.

    ```javascript
    // male
    [1, 2, 3].map(x => x * x);

    // bene
    [1, 2, 3].map((x) => x * x);

    // male
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // bene
    [1, 2, 3].map((number) => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // male
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // bene
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--confusing"></a><a name="8.5"></a>
  - [8.5](#arrows--confusing) Evitare di confondere la sintassi delle funzioni arrow (`=>`) con gli operatori di confronto (`<=`, `>=`). eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // male
    const itemHeight = (item) => item.height <= 256 ? item.largeSize : item.smallSize;

    // male
    const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

    // bene
    const itemHeight = (item) => (item.height <= 256 ? item.largeSize : item.smallSize);

    // bene
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height <= 256 ? largeSize : smallSize;
    };
    ```

  <a name="whitespace--implicit-arrow-linebreak"></a>
  - [8.6](#whitespace--implicit-arrow-linebreak) Imporre la posizione dei corpi delle funzioni freccia con return impliciti. eslint: [`implicit-arrow-linebreak`](https://eslint.org/docs/rules/implicit-arrow-linebreak)

    ```javascript
    // male
    (foo) =>
      bar;

    (foo) =>
      (bar);

    // bene
    (foo) => bar;
    (foo) => (bar);
    (foo) => (
       bar
    )
    ```

**[⬆ torna all'inizio](#sommario)**

## Classi & Costruttori

  <a name="constructors--use-class"></a><a name="9.1"></a>
  - [9.1](#constructors--use-class) Usare sempre `class`. Evitare di manipolare direttamente `prototype`.

    > Perche? La sintassi `class` è più concisa e più facile da ragionare.

    ```javascript
    // male
    function Queue(contents = []) {
      this.queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };

    // bene
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents];
      }
      pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      }
    }
    ```

  <a name="constructors--extends"></a><a name="9.2"></a>
  - [9.2](#constructors--extends) Usare `extends` per ereditarietà.

    > Perche? E' un modo built-in per ereditare la funzionalità di prototipo senza rompere `instanceof`.

    ```javascript
    // male
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this.queue[0];
    };

    // bene
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0];
      }
    }
    ```

  <a name="constructors--chaining"></a><a name="9.3"></a>
  - [9.3](#constructors--chaining) I metodi possono ritornare `this` per facilitare il concatenamento dei metodi.

    ```javascript
    // male
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // bene
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  <a name="constructors--tostring"></a><a name="9.4"></a>
  - [9.4](#constructors--tostring) Va bene scrivere un metodo `toString()` personalizzato, solo assicurarsi che funzioni con successo e non causi effetti collaterali.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) Le classi hanno un costruttore predefinito se non ne viene specificato uno. Una funzione costruttore vuota o una che semplicemente delega a una classe genitore non è necessaria. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // male
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // male
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // bene
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>
  - [9.6](#classes--no-duplicate-members) Evitare duplicati di membri di una classe. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > Perche? Dichiarazioni di membri di classe duplicati fanno preferire in modo silente l'ultimo - avere duplicati è quasi certamente un bug.

    ```javascript
    // male
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // bene
    class Foo {
      bar() { return 1; }
    }

    // bene
    class Foo {
      bar() { return 2; }
    }
    ```

  <a name="classes--methods-use-this"></a>
  - [9.7](#classes--methods-use-this) I metodi di classe dovrebbero usare `this` oppure essere resi statici a meno che una libreria esterna o un framework richieda l'utilizzo specifici metodi non statici. Essere un metodo di istanza dovrebbe indicare che esso si comporta diversamente in base alle proprietà del ricevente. eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

    ```javascript
    // male
    class Foo {
      bar() {
        console.log('bar');
      }
    }

    // bene - questo è usato
    class Foo {
      bar() {
        console.log(this.bar);
      }
    }

    // bene - costruttore è esente
    class Foo {
      constructor() {
        // ...
      }
    }

    // bene - I metodi statici non dovrebbero usare this
    class Foo {
      static bar() {
        console.log('bar');
      }
    }
    ```

**[⬆ torna all'inizio](#sommario)**

## Moduli

  <a name="modules--use-them"></a><a name="10.1"></a>
  - [10.1](#modules--use-them) Usare sempre (`import`/`export`) per importare/esportare i moduli rispetto a un sistema di moduli non standard. Si può sempre eseguire un transpile verso il proprio sistema di moduli preferito.

    > Perche? I moduli sono il futuro, si inizi a usare il futuro ora.

    ```javascript
    // male
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="10.2"></a>
  - [10.2](#modules--no-wildcard) Do not usare wildcard con gli import.

    > Perche? Ciò assicura di avere un singolo export predefinito.

    ```javascript
    // male
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // bene
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  <a name="modules--no-export-from-import"></a><a name="10.3"></a>
  - [10.3](#modules--no-export-from-import) Non esportare direttamente da un import.

    > Perche? Anche se l'istruzione in una riga è concisa, avere un chiaro modo di importare ed esportare rende le cosi consistenti.

    ```javascript
    // male
    // filename es6.js
    export { es6 as default } from './AirbnbStyleGuide';

    // bene
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"></a>
  - [10.4](#modules--no-duplicate-imports) Importare da un percorso in un solo posto.
 eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
    
    > Perche? Avere molteplici righe che importano dallo stesso percorso può rendere il codice più difficile da mantenere.

    ```javascript
    // male
    import foo from 'foo';
    // … some other imports … //
    import { named1, named2 } from 'foo';

    // bene
    import foo, { named1, named2 } from 'foo';

    // bene
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"></a>
  - [10.5](#modules--no-mutable-exports) Do not export mutable bindings.
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > Perche? Mutation should be avoided in general, but in particular when exporting mutable bindings. While this technique may be needed for some special cases, in general, only constant references should be exported.

    ```javascript
    // male
    let foo = 3;
    export { foo };

    // bene
    const foo = 3;
    export { foo };
    ```

  <a name="modules--prefer-default-export"></a>
  - [10.6](#modules--prefer-default-export) In modules with a single export, prefer default export over named export.
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
    > Perche? To encourage more files that only ever export one thing, which is better for readability and maintainability.

    ```javascript
    // male
    export function foo() {}

    // bene
    export default function foo() {}
    ```

  <a name="modules--imports-first"></a>
  - [10.7](#modules--imports-first) Put all `import`s above non-import statements.
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > Perche? Since `import`s are hoisted, keeping them all at the top prevents surprising behavior.

    ```javascript
    // male
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // bene
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  <a name="modules--multiline-imports-over-newlines"></a>
  - [10.8](#modules--multiline-imports-over-newlines) Multiline imports should be indented just like multiline array and object literals.
 eslint: [`object-curly-newline`](https://eslint.org/docs/rules/object-curly-newline)

    > Perche? The curly braces follow the same indentation rules as every other curly brace block in the style guide, as do the trailing commas.

    ```javascript
    // male
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // bene
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path';
    ```

  <a name="modules--no-webpack-loader-syntax"></a>
  - [10.9](#modules--no-webpack-loader-syntax) Disallow Webpack loader syntax in module import statements.
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > Perche? Since using Webpack syntax in the imports couples the code to a module bundler. Prefer using the loader syntax in `webpack.config.js`.

    ```javascript
    // male
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // bene
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

  <a name="modules--import-extensions"></a>
  - [10.10](#modules--import-extensions) Do not include JavaScript filename extensions
 eslint: [`import/extensions`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/extensions.md)
    > Perche? Including extensions inhibits refactoring, and inappropriately hardcodes implementation details of the module you're importing in every consumer.

    ```javascript
    // male
    import foo from './foo.js';
    import bar from './bar.jsx';
    import baz from './baz/index.jsx';

    // bene
    import foo from './foo';
    import bar from './bar';
    import baz from './baz';
    ```

**[⬆ torna all'inizio](#sommario)**

## Iterators and Generators

  <a name="iterators--nope"></a><a name="11.1"></a>
  - [11.1](#iterators--nope) Don’t Usare iterators. Prefer JavaScript’s higher-order functions instead of loops like `for-in` or `for-of`. eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

    > Perche? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side effects.

    > Usare `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... to iterate over arrays, and `Object.keys()` / `Object.values()` / `Object.entries()` to produce arrays so you can iterate over objects.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // male
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }
    sum === 15;

    // bene
    let sum = 0;
    numbers.forEach((num) => {
      sum += num;
    });
    sum === 15;

    // best (Usare the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;

    // male
    const increasedByOne = [];
    for (let i = 0; i < numbers.length; i++) {
      increasedByOne.push(numbers[i] + 1);
    }

    // bene
    const increasedByOne = [];
    numbers.forEach((num) => {
      increasedByOne.push(num + 1);
    });

    // best (keeping it functional)
    const increasedByOne = numbers.map((num) => num + 1);
    ```

  <a name="generators--nope"></a><a name="11.2"></a>
  - [11.2](#generators--nope) Don’t Usare generators for now.

    > Perche? They don’t transpile well to ES5.

  <a name="generators--spacing"></a>
  - [11.3](#generators--spacing) If you must Usare generators, or if you disregard [our advice](#generators--nope), make sure their function signature is spaced properly. eslint: [`generator-star-spacing`](https://eslint.org/docs/rules/generator-star-spacing)

    > Perche? `function` and `*` are part of the same conceptual keyword - `*` is not a modifier for `function`, `function*` is a unique construct, different from `function`.

    ```javascript
    // male
    function * foo() {
      // ...
    }

    // male
    const bar = function * () {
      // ...
    };

    // male
    const baz = function *() {
      // ...
    };

    // male
    const quux = function*() {
      // ...
    };

    // male
    function*foo() {
      // ...
    }

    // male
    function *foo() {
      // ...
    }

    // very bad
    function
    *
    foo() {
      // ...
    }

    // very bad
    const wat = function
    *
    () {
      // ...
    };

    // bene
    function* foo() {
      // ...
    }

    // bene
    const foo = function* () {
      // ...
    };
    ```

**[⬆ torna all'inizio](#sommario)**

## Properties

  <a name="properties--dot"></a><a name="12.1"></a>
  - [12.1](#properties--dot) Usare dot notation when accessing properties. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // male
    const isJedi = luke['jedi'];

    // bene
    const isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="12.2"></a>
  - [12.2](#properties--bracket) Usare bracket notation `[]` when accessing properties with a variable.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

  <a name="es2016-properties--exponentiation-operator"></a>
  - [12.3](#es2016-properties--exponentiation-operator) Usare exponentiation operator `**` when calculating exponentiations. eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties).

    ```javascript
    // male
    const binary = Math.pow(2, 10);

    // bene
    const binary = 2 ** 10;
    ```

**[⬆ torna all'inizio](#sommario)**

## Variables

  <a name="variables--const"></a><a name="13.1"></a>
  - [13.1](#variables--const) Always Usare `const` or `let` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that. eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // male
    superPower = new SuperPower();

    // bene
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="13.2"></a>
  - [13.2](#variables--one-const) Usare one `const` or `let` declaration per variable or assignment. eslint: [`one-var`](https://eslint.org/docs/rules/one-var)

    > Perche? It’s easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs. You can also step through each declaration with the debugger, instead of jumping through all of them at once.

    ```javascript
    // male
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // male
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // bene
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  <a name="variables--const-let-group"></a><a name="13.3"></a>
  - [13.3](#variables--const-let-group) Group all your `const`s and then group all your `let`s.

    > Perche? This is helpful when later on you might need to assign a variable depending on one of the previously assigned variables.

    ```javascript
    // male
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // male
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // bene
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  <a name="variables--define-where-used"></a><a name="13.4"></a>
  - [13.4](#variables--define-where-used) Assign variables where you need them, but place them in a reasonable place.

    > Perche? `let` and `const` are block scoped and not function scoped.

    ```javascript
    // male - unnecessary function call
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // bene
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }
    ```

  <a name="variables--no-chain-assignment"></a><a name="13.5"></a>
  - [13.5](#variables--no-chain-assignment) Don’t chain variable assignments. eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

    > Perche? Chaining variable assignments creates implicit global variables.

    ```javascript
    // male
    (function example() {
      // JavaScript interprets this as
      // let a = ( b = ( c = 1 ) );
      // The let keyword only applies to variable a; variables b and c become
      // global variables.
      let a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // bene
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError

    // the same applies for `const`
    ```

  <a name="variables--unary-increment-decrement"></a><a name="13.6"></a>
  - [13.6](#variables--unary-increment-decrement) Avoid using unary increments and decrements (`++`, `--`). eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

    > Perche? Per the eslint documentation, unary increment and decrement statements are subject to automatic semicolon insertion and can caUsare silent errors with incrementing or decrementing values within an application. It is also more expressive to mutate your values with statements like `num += 1` instead of `num++` or `num ++`. Disallowing unary increment and decrement statements also prevents you from pre-incrementing/pre-decrementing values unintentionally which can also caUsare unexpected behavior in your programs.

    ```javascript
    // male

    const array = [1, 2, 3];
    let num = 1;
    num++;
    --num;

    let sum = 0;
    let truthyCount = 0;
    for (let i = 0; i < array.length; i++) {
      let value = array[i];
      sum += value;
      if (value) {
        truthyCount++;
      }
    }

    // bene

    const array = [1, 2, 3];
    let num = 1;
    num += 1;
    num -= 1;

    const sum = array.reduce((a, b) => a + b, 0);
    const truthyCount = array.filter(Boolean).length;
    ```

<a name="variables--linebreak"></a>
  - [13.7](#variables--linebreak) Avoid linebreaks before or after `=` in an assignment. If your assignment violates [`max-len`](https://eslint.org/docs/rules/max-len), surround the value in parens. eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak).

    > Perche? Linebreaks surrounding `=` can obfuscate the value of an assignment.

    ```javascript
    // male
    const foo =
      superLongLongLongLongLongLongLongLongFunctionName();

    // male
    const foo
      = 'superLongLongLongLongLongLongLongLongString';

    // bene
    const foo = (
      superLongLongLongLongLongLongLongLongFunctionName()
    );

    // bene
    const foo = 'superLongLongLongLongLongLongLongLongString';
    ```

<a name="variables--no-unused-vars"></a>
  - [13.8](#variables--no-unused-vars) Disallow unused variables. eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

    > Perche? Variables that are declared and not used anywhere in the code are most likely an error due to incomplete refactoring. Such variables take up space in the code and can lead to confusion by readers.

    ```javascript
    // male

    var some_unused_var = 42;

    // Write-only variables are not considered as used.
    var y = 10;
    y = 5;

    // A read for a modification of itself is not considered as used.
    var z = 0;
    z = z + 1;

    // Unused function arguments.
    function getX(x, y) {
        return x;
    }

    // bene

    function getXPlusY(x, y) {
      return x + y;
    }

    var x = 1;
    var y = a + 2;

    alert(getXPlusY(x, y));

    // 'type' is ignored even if unused becaUsare it has a rest property sibling.
    // This is a form of extracting an object that omits the specified keys.
    var { type, ...coords } = data;
    // 'coords' is now the 'data' object without its 'type' property.
    ```

**[⬆ torna all'inizio](#sommario)**

## Hoisting

  <a name="hoisting--about"></a><a name="14.1"></a>
  - [14.1](#hoisting--about) `var` declarations get hoisted to the top of their closest enclosing function scope, their assignment does not. `const` and `let` declarations are blessed with a new concept called [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#temporal_dead_zone_tdz). It’s important to know why [typeof is no longer safe](https://web.archive.org/web/20200121061528/http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

    ```javascript
    // we know this wouldn’t work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // the interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // using const and let
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  <a name="hoisting--anon-expressions"></a><a name="14.2"></a>
  - [14.2](#hoisting--anon-expressions) Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  <a name="hoisting--named-expresions"></a><a name="hoisting--named-expressions"></a><a name="14.3"></a>
  - [14.3](#hoisting--named-expressions) Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      };
    }
    ```

  <a name="hoisting--declarations"></a><a name="14.4"></a>
  - [14.4](#hoisting--declarations) Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](https://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](https://www.adequatelygood.com/).

**[⬆ torna all'inizio](#sommario)**

## Comparison Operators & Equality

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - [15.1](#comparison--eqeqeq) Usare `===` and `!==` over `==` and `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq)

  <a name="comparison--if"></a><a name="15.2"></a>
  - [15.2](#comparison--if) Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:

    - **Objects** evaluate to **true**
    - **Undefined** evaluates to **false**
    - **Null** evaluates to **false**
    - **Booleans** evaluate to **the value of the boolean**
    - **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    - **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0] && []) {
      // true
      // an array (even an empty one) is an object, objects will evaluate to true
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) Usare shortcuts for booleans, but explicit comparisons for strings and numbers.

    ```javascript
    // male
    if (isValid === true) {
      // ...
    }

    // bene
    if (isValid) {
      // ...
    }

    // male
    if (name) {
      // ...
    }

    // bene
    if (name !== '') {
      // ...
    }

    // male
    if (collection.length) {
      // ...
    }

    // bene
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--moreinfo"></a><a name="15.4"></a>
  - [15.4](#comparison--moreinfo) For more information see [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  - [15.5](#comparison--switch-blocks) Usare braces to create blocks in `case` and `default` clauses that contain lexical declarations (e.g. `let`, `const`, `function`, and `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations)

    > Perche? Lexical declarations are visible in the entire `switch` block but only get initialized when assigned, which only happens when its `case` is reached. This causes problems when multiple `case` clauses attempt to define the same thing.

    ```javascript
    // male
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {
          // ...
        }
        break;
      default:
        class C {}
    }

    // bene
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {
          // ...
        }
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>
  - [15.6](#comparison--nested-ternaries) Ternaries should not be nested and generally be single line expressions. eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary)

    ```javascript
    // male
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // split into 2 separated ternary expressions
    const maybeNull = value1 > value2 ? 'baz' : null;

    // better
    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // best
    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
  - [15.7](#comparison--unneeded-ternary) Avoid unneeded ternary statements. eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary)

    ```javascript
    // male
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // bene
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

  <a name="comparison--no-mixed-operators"></a>
  - [15.8](#comparison--no-mixed-operators) When mixing operators, enclose them in parentheses. The only exception is the standard arithmetic operators: `+`, `-`, and `**` since their precedence is broadly understood. We recommend enclosing `/` and `*` in parentheses becaUsare their precedence can be ambiguous when they are mixed.
  eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators)

    > Perche? This improves readability and clarifies the developer’s intention.

    ```javascript
    // male
    const foo = a && b < 0 || c > 0 || d + 1 === 0;

    // male
    const bar = a ** b - 5 % d;

    // male
    // one may be confused into thinking (a || b) && c
    if (a || b && c) {
      return d;
    }

    // male
    const bar = a + b / c * d;

    // bene
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

    // bene
    const bar = a ** b - (5 % d);

    // bene
    if (a || (b && c)) {
      return d;
    }

    // bene
    const bar = a + (b / c) * d;
    ```

**[⬆ torna all'inizio](#sommario)**

## Blocks

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces) Usare braces with all multiline blocks. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // male
    if (test)
      return false;

    // bene
    if (test) return false;

    // bene
    if (test) {
      return false;
    }

    // male
    function foo() { return false; }

    // bene
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) If you’re using multiline blocks with `if` and `else`, put `else` on the same line as your `if` block’s closing brace. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style)

    ```javascript
    // male
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // bene
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

  <a name="blocks--no-else-return"></a><a name="16.3"></a>
  - [16.3](#blocks--no-else-return) If an `if` block always executes a `return` statement, the subsequent `else` block is unnecessary. A `return` in an `else if` block following an `if` block that contains a `return` can be separated into multiple `if` blocks. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

    ```javascript
    // male
    function foo() {
      if (x) {
        return x;
      } else {
        return y;
      }
    }

    // male
    function cats() {
      if (x) {
        return x;
      } else if (y) {
        return y;
      }
    }

    // male
    function dogs() {
      if (x) {
        return x;
      } else {
        if (y) {
          return y;
        }
      }
    }

    // bene
    function foo() {
      if (x) {
        return x;
      }

      return y;
    }

    // bene
    function cats() {
      if (x) {
        return x;
      }

      if (y) {
        return y;
      }
    }

    // bene
    function dogs(x) {
      if (x) {
        if (z) {
          return y;
        }
      } else {
        return z;
      }
    }
    ```

**[⬆ torna all'inizio](#sommario)**

## Istruzioni di Controllo

  <a name="control-statements"></a>
  - [17.1](#control-statements) In case your control statement (`if`, `while` etc.) gets too long or exceeds the maximum line length, each (grouped) condition could be put into a new line. The logical operator should begin the line.

    > Perche? Requiring operators at the beginning of the line keeps the operators aligned and follows a pattern similar to method chaining. This also improves readability by making it easier to visually follow complex logic.

    ```javascript
    // male
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // male
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // male
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // male
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // bene
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // bene
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // bene
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

  <a name="control-statement--value-selection"></a><a name="control-statements--value-selection"></a>
  - [17.2](#control-statements--value-selection) Don't Usare selection operators in place of control statements.

    ```javascript
    // male
    !isRunning && startRunning();

    // bene
    if (!isRunning) {
      startRunning();
    }
    ```

**[⬆ torna all'inizio](#sommario)**

## Comments

  <a name="comments--multiline"></a><a name="17.1"></a>
  - [18.1](#comments--multiline) Usare `/** ... */` for multiline comments.

    ```javascript
    // male
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // bene
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="17.2"></a>
  - [18.2](#comments--singleline) Usare `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it’s on the first line of a block.

    ```javascript
    // male
    const active = true;  // is current tab

    // bene
    // is current tab
    const active = true;

    // male
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // bene
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  <a name="comments--spaces"></a>
  - [18.3](#comments--spaces) Start all comments with a space to make it easier to read. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // male
    //is current tab
    const active = true;

    // bene
    // is current tab
    const active = true;

    // male
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }

    // bene
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="17.3"></a>
  - [18.4](#comments--actionitems) Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented. These are different than regular comments becaUsare they are actionable. The actions are `FIXME: -- need to figure this out` or `TODO: -- need to implement`.

  <a name="comments--fixme"></a><a name="17.4"></a>
  - [18.5](#comments--fixme) Usare `// FIXME:` to annotate problems.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn’t Usare a global here
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="17.5"></a>
  - [18.6](#comments--todo) Usare `// TODO:` to annotate solutions to problems.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

**[⬆ torna all'inizio](#sommario)**

## Whitespace

  <a name="whitespace--spaces"></a><a name="18.1"></a>
  - [19.1](#whitespace--spaces) Usare soft tabs (space character) set to 2 spaces. eslint: [`indent`](https://eslint.org/docs/rules/indent)

    ```javascript
    // male
    function foo() {
    ∙∙∙∙let name;
    }

    // male
    function bar() {
    ∙let name;
    }

    // bene
    function baz() {
    ∙∙let name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="18.2"></a>
  - [19.2](#whitespace--before-blocks) Place 1 space before the leading brace. eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    ```javascript
    // male
    function test(){
      console.log('test');
    }

    // bene
    function test() {
      console.log('test');
    }

    // male
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // bene
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="18.3"></a>
  - [19.3](#whitespace--around-keywords) Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space between the argument list and the function name in function calls and declarations. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing)

    ```javascript
    // male
    if(isJedi) {
      fight ();
    }

    // bene
    if (isJedi) {
      fight();
    }

    // male
    function fight () {
      console.log ('Swooosh!');
    }

    // bene
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="18.4"></a>
  - [19.4](#whitespace--infix-ops) Set off operators with spaces. eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops)

    ```javascript
    // male
    const x=y+5;

    // bene
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="18.5"></a>
  - [19.5](#whitespace--newline-at-end) End files with a single newline character. eslint: [`eol-last`](https://eslint.org/docs/rules/eol-last)

    ```javascript
    // male
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;
    ```

    ```javascript
    // male
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ↵
    ```

    ```javascript
    // bene
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ```

  <a name="whitespace--chains"></a><a name="18.6"></a>
  - [19.6](#whitespace--chains) Usare indentation when making long method chains (more than 2 method chains). Usare a leading dot, which
    emphasizes that the line is a method call, not a new statement. eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // male
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // male
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // bene
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // male
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // bene
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // bene
    const leds = stage.selectAll('.led').data(data);
    const svg = leds.enter().append('svg:svg');
    svg.classed('led', true).attr('width', (radius + margin) * 2);
    const g = svg.append('svg:g');
    g.attr('transform', `translate(${radius + margin},${radius + margin})`).call(tron.led);
    ```

  <a name="whitespace--after-blocks"></a><a name="18.7"></a>
  - [19.7](#whitespace--after-blocks) Leave a blank line after blocks and before the next statement.

    ```javascript
    // male
    if (foo) {
      return bar;
    }
    return baz;

    // bene
    if (foo) {
      return bar;
    }

    return baz;

    // male
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // bene
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // male
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // bene
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="18.8"></a>
  - [19.8](#whitespace--padded-blocks) Do not pad your blocks with blank lines. eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks)

    ```javascript
    // male
    function bar() {

      console.log(foo);

    }

    // male
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // male
    class Foo {

      constructor(bar) {
        this.bar = bar;
      }
    }

    // bene
    function bar() {
      console.log(foo);
    }

    // bene
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--no-multiple-blanks"></a>
  - [19.9](#whitespace--no-multiple-blanks) Do not Usare multiple blank lines to pad your code. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // male
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName;


        this.email = email;


        this.setAge(birthday);
      }


      setAge(birthday) {
        const today = new Date();


        const age = this.getAge(today, birthday);


        this.age = age;
      }


      getAge(today, birthday) {
        // ..
      }
    }

    // bene
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName;
        this.email = email;
        this.setAge(birthday);
      }

      setAge(birthday) {
        const today = new Date();
        const age = getAge(today, birthday);
        this.age = age;
      }

      getAge(today, birthday) {
        // ..
      }
    }
    ```

  <a name="whitespace--in-parens"></a><a name="18.9"></a>
  - [19.10](#whitespace--in-parens) Do not add spaces inside parentheses. eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens)

    ```javascript
    // male
    function bar( foo ) {
      return foo;
    }

    // bene
    function bar(foo) {
      return foo;
    }

    // male
    if ( foo ) {
      console.log(foo);
    }

    // bene
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="18.10"></a>
  - [19.11](#whitespace--in-brackets) Do not add spaces inside brackets. eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing)

    ```javascript
    // male
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // bene
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="18.11"></a>
  - [19.12](#whitespace--in-braces) Add spaces inside curly braces. eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing)

    ```javascript
    // male
    const foo = {clark: 'kent'};

    // bene
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="18.12"></a>
  - [19.13](#whitespace--max-len) Avoid having lines of code that are longer than 100 characters (including whitespace). Note: per [above](#strings--line-length), long strings are exempt from this rule, and should not be broken up. eslint: [`max-len`](https://eslint.org/docs/rules/max-len)

    > Perche? This ensures readability and maintainability.

    ```javascript
    // male
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // male
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // bene
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // bene
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

  <a name="whitespace--block-spacing"></a>
  - [19.14](#whitespace--block-spacing) Require consistent spacing inside an open block token and the next token on the same line. This rule also enforces consistent spacing inside a close block token and previous token on the same line. eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // male
    function foo() {return true;}
    if (foo) { bar = 0;}

    // bene
    function foo() { return true; }
    if (foo) { bar = 0; }
    ```

  <a name="whitespace--comma-spacing"></a>
  - [19.15](#whitespace--comma-spacing) Avoid spaces before commas and require a space after commas. eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

    ```javascript
    // male
    var foo = 1,bar = 2;
    var arr = [1 , 2];

    // bene
    var foo = 1, bar = 2;
    var arr = [1, 2];
    ```

  <a name="whitespace--computed-property-spacing"></a>
  - [19.16](#whitespace--computed-property-spacing) Enforce spacing inside of computed property brackets. eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

    ```javascript
    // male
    obj[foo ]
    obj[ 'foo']
    var x = {[ b ]: a}
    obj[foo[ bar ]]

    // bene
    obj[foo]
    obj['foo']
    var x = { [b]: a }
    obj[foo[bar]]
    ```

  <a name="whitespace--func-call-spacing"></a>
  - [19.17](#whitespace--func-call-spacing) Avoid spaces between functions and their invocations. eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

    ```javascript
    // male
    func ();

    func
    ();

    // bene
    func();
    ```

  <a name="whitespace--key-spacing"></a>
  - [19.18](#whitespace--key-spacing) Enforce spacing between keys and values in object literal properties. eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // male
    var obj = { foo : 42 };
    var obj2 = { foo:42 };

    // bene
    var obj = { foo: 42 };
    ```

  <a name="whitespace--no-trailing-spaces"></a>
  - [19.19](#whitespace--no-trailing-spaces) Avoid trailing spaces at the end of lines. eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

  <a name="whitespace--no-multiple-empty-lines"></a>
  - [19.20](#whitespace--no-multiple-empty-lines) Avoid multiple empty lines, only allow one newline at the end of files, and avoid a newline at the beginning of files. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // male - multiple empty lines
    var x = 1;


    var y = 2;

    // male - 2+ newlines at end of file
    var x = 1;
    var y = 2;


    // male - 1+ newline(s) at beginning of file

    var x = 1;
    var y = 2;

    // bene
    var x = 1;
    var y = 2;

    ```
    <!-- markdownlint-enable MD012 -->

**[⬆ torna all'inizio](#sommario)**

## Commas

  <a name="commas--leading-trailing"></a><a name="19.1"></a>
  - [20.1](#commas--leading-trailing) Leading commas: **Nope.** eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style)

    ```javascript
    // male
    const story = [
        once
      , upon
      , aTime
    ];

    // bene
    const story = [
      once,
      upon,
      aTime,
    ];

    // male
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // bene
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  <a name="commas--dangling"></a><a name="19.2"></a>
  - [20.2](#commas--dangling) Additional trailing comma: **Yup.** eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle)

    > Perche? This leads to cleaner git diffs. Also, transpilers like Babel will remove the additional trailing comma in the transpiled code which means you don’t have to worry about the [trailing comma problem](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) in legacy browsers.

    ```diff
    // male - git diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // bene - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };
    ```

    ```javascript
    // male
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // bene
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];

    // male
    function createHero(
      firstName,
      lastName,
      inventorOf
    ) {
      // does nothing
    }

    // bene
    function createHero(
      firstName,
      lastName,
      inventorOf,
    ) {
      // does nothing
    }

    // bene (note that a comma must not appear after a "rest" element)
    function createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    ) {
      // does nothing
    }

    // male
    createHero(
      firstName,
      lastName,
      inventorOf
    );

    // bene
    createHero(
      firstName,
      lastName,
      inventorOf,
    );

    // bene (note that a comma must not appear after a "rest" element)
    createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    );
    ```

**[⬆ torna all'inizio](#sommario)**

## Semicolons

  <a name="semicolons--required"></a><a name="20.1"></a>
  - [21.1](#semicolons--required) **Yup.** eslint: [`semi`](https://eslint.org/docs/rules/semi)

    > Perche? When JavaScript encounters a line break without a semicolon, it uses a set of rules called [Automatic Semicolon Insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) to determine whether it should regard that line break as the end of a statement, and (as the name implies) place a semicolon into your code before the line break if it thinks so. ASI contains a few eccentric behaviors, though, and your code will break if JavaScript misinterprets your line break. These rules will become more complicated as new features become a part of JavaScript. Explicitly terminating your statements and configuring your linter to catch missing semicolons will help prevent you from encountering issues.

    ```javascript
    // male - raises exception
    const luke = {}
    const leia = {}
    [luke, leia].forEach((jedi) => jedi.father = 'vader')

    // male - raises exception
    const reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // male - returns `undefined` instead of the value on the next line - always happens when `return` is on a line by itself becaUsare of ASI!
    function foo() {
      return
        'search your feelings, you know it to be foo'
    }

    // bene
    const luke = {};
    const leia = {};
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader';
    });

    // bene
    const reaction = "No! That’s impossible!";
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }());

    // bene
    function foo() {
      return 'search your feelings, you know it to be foo';
    }
    ```

    [Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ torna all'inizio](#sommario)**

## Type Casting & Coercion

  <a name="coercion--explicit"></a><a name="21.1"></a>
  - [22.1](#coercion--explicit) Perform type coercion at the beginning of the statement.

  <a name="coercion--strings"></a><a name="21.2"></a>
  - [22.2](#coercion--strings) Strings: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    // => this.reviewScore = 9;

    // male
    const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

    // male
    const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

    // male
    const totalScore = this.reviewScore.toString(); // isn’t guaranteed to return a string

    // bene
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>
  - [22.3](#coercion--numbers) Numbers: Usare `Number` for type casting and `parseInt` always with a radix for parsing strings. eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    > Perche? The `parseInt` function produces an integer value dictated by interpretation of the contents of the string argument according to the specified radix. Leading whitespace in string is ignored. If radix is `undefined` or `0`, it is assumed to be `10` except when the number begins with the character pairs `0x` or `0X`, in which case a radix of 16 is assumed. This differs from ECMAScript 3, which merely discouraged (but allowed) octal interpretation. Many implementations have not adopted this behavior as of 2013. And, becaUsare older browsers must be supported, always specify a radix.

    ```javascript
    const inputValue = '4';

    // male
    const val = new Number(inputValue);

    // male
    const val = +inputValue;

    // male
    const val = inputValue >> 0;

    // male
    const val = parseInt(inputValue);

    // bene
    const val = Number(inputValue);

    // bene
    const val = parseInt(inputValue, 10);
    ```

  <a name="coercion--comment-deviations"></a><a name="21.4"></a>
  - [22.4](#coercion--comment-deviations) If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to Usare Bitshift for [performance reasons](https://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you’re doing.

    ```javascript
    // bene
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    const val = inputValue >> 0;
    ```

  <a name="coercion--bitwise"></a><a name="21.5"></a>
  - [22.5](#coercion--bitwise) **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](https://es5.github.io/#x4.3.19), but bitshift operations always return a 32-bit integer ([source](https://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0; // => 2147483647
    2147483648 >> 0; // => -2147483648
    2147483649 >> 0; // => -2147483647
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>
  - [22.6](#coercion--booleans) Booleans: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const age = 0;

    // male
    const hasAge = new Boolean(age);

    // bene
    const hasAge = Boolean(age);

    // best
    const hasAge = !!age;
    ```

**[⬆ torna all'inizio](#sommario)**

## Convenzioni di Denominazione

  <a name="naming--descriptive"></a><a name="22.1"></a>
  - [23.1](#naming--descriptive) Avoid single letter names. Be descriptive with your naming. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

    ```javascript
    // male
    function q() {
      // ...
    }

    // bene
    function query() {
      // ...
    }
    ```

  <a name="naming--camelCase"></a><a name="22.2"></a>
  - [23.2](#naming--camelCase) Usare camelCase when naming objects, functions, and instances. eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase)

    ```javascript
    // male
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // bene
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="22.3"></a>
  - [23.3](#naming--PascalCase) Usare PascalCase only when naming constructors or classes. eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap)

    ```javascript
    // male
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // bene
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  <a name="naming--leading-underscore"></a><a name="22.4"></a>
  - [23.4](#naming--leading-underscore) Do not Usare trailing or leading underscores. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle)

    > Perche? JavaScript does not have the concept of privacy in terms of properties or methods. Although a leading underscore is a common convention to mean “private”, in fact, these properties are fully public, and as such, are part of your public API contract. This convention might lead developers to wrongly think that a change won’t count as breaking, or that tests aren’t needed. tl;dr: if you want something to be “private”, it must not be observably present.

    ```javascript
    // male
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // bene
    this.firstName = 'Panda';

    // bene, in environments where WeakMaps are available
    // see https://kangax.github.io/compat-table/es6/#test-WeakMap
    const firstNames = new WeakMap();
    firstNames.set(this, 'Panda');
    ```

  <a name="naming--self-this"></a><a name="22.5"></a>
  - [23.5](#naming--self-this) Don’t save references to `this`. Usare arrow functions or [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

    ```javascript
    // male
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // male
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // bene
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  <a name="naming--filename-matches-export"></a><a name="22.6"></a>
  - [23.6](#naming--filename-matches-export) A base filename should exactly match the name of its default export.

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // file 2 contents
    export default function fortyTwo() { return 42; }

    // file 3 contents
    export default function insideDirectory() {}

    // in some other file
    // male
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // male
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // bene
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="22.7"></a>
  - [23.7](#naming--camelCase-default-export) Usare camelCase when you export-default a function. Your filename should be identical to your function’s name.

    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide;
    ```

  <a name="naming--PascalCase-singleton"></a><a name="22.8"></a>
  - [23.8](#naming--PascalCase-singleton) Usare PascalCase when you export a constructor / class / singleton / function library / bare object.

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      },
    };

    export default AirbnbStyleGuide;
    ```

  <a name="naming--Acronyms-and-Initialisms"></a>
  - [23.9](#naming--Acronyms-and-Initialisms) Acronyms and initialisms should always be all uppercased, or all lowercased.

    > Perche? Names are for readability, not to appease a computer algorithm.

    ```javascript
    // male
    import SmsContainer from './containers/SmsContainer';

    // male
    const HttpRequests = [
      // ...
    ];

    // bene
    import SMSContainer from './containers/SMSContainer';

    // bene
    const HTTPRequests = [
      // ...
    ];

    // also good
    const httpRequests = [
      // ...
    ];

    // best
    import TextMessageContainer from './containers/TextMessageContainer';

    // best
    const requests = [
      // ...
    ];
    ```

  <a name="naming--uppercase"></a>
  - [23.10](#naming--uppercase) You may optionally uppercase a constant only if it (1) is exported, (2) is a `const` (it can not be reassigned), and (3) the programmer can trust it (and its nested properties) to never change.

    > Perche? This is an additional tool to assist in situations where the programmer would be unsure if a variable might ever change. UPPERCASE_VARIABLES are letting the programmer know that they can trust the variable (and its properties) not to change.
    - What about all `const` variables? - This is unnecessary, so uppercasing should not be used for constants within a file. It should be used for exported constants however.
    - What about exported objects? - Uppercase at the top level of export (e.g. `EXPORTED_OBJECT.key`) and maintain that all nested properties do not change.

    ```javascript
    // male
    const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

    // male
    export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

    // male
    export let REASSIGNABLE_VARIABLE = 'do not Usare let with uppercase variables';

    // ---

    // allowed but does not supply semantic value
    export const apiKey = 'SOMEKEY';

    // better in most cases
    export const API_KEY = 'SOMEKEY';

    // ---

    // male - unnecessarily uppercases key while adding no semantic value
    export const MAPPING = {
      KEY: 'value'
    };

    // bene
    export const MAPPING = {
      key: 'value'
    };
    ```

**[⬆ torna all'inizio](#sommario)**

## Accessors

  <a name="accessors--not-required"></a><a name="23.1"></a>
  - [24.1](#accessors--not-required) Accessor functions for properties are not required.

  <a name="accessors--no-getters-setters"></a><a name="23.2"></a>
  - [24.2](#accessors--no-getters-setters) Do not Usare JavaScript getters/setters as they caUsare unexpected side effects and are harder to test, maintain, and reason about. Instead, if you do make accessor functions, Usare `getVal()` and `setVal('hello')`.

    ```javascript
    // male
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // bene
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="23.3"></a>
  - [24.3](#accessors--boolean-prefix) If the property/method is a `boolean`, Usare `isVal()` or `hasVal()`.

    ```javascript
    // male
    if (!dragon.age()) {
      return false;
    }

    // bene
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  <a name="accessors--consistent"></a><a name="23.4"></a>
  - [24.4](#accessors--consistent) It’s okay to create `get()` and `set()` functions, but be consistent.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ torna all'inizio](#sommario)**

## Eventi

  <a name="events--hash"></a><a name="24.1"></a>
  - [25.1](#events--hash) When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass an object literal (also known as a "hash") instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```javascript
    // male
    $(this).trigger('listingUpdated', listing.id);

    // ...

    $(this).on('listingUpdated', (e, listingID) => {
      // do something with listingID
    });
    ```

    prefer:

    ```javascript
    // bene
    $(this).trigger('listingUpdated', { listingID: listing.id });

    // ...

    $(this).on('listingUpdated', (e, data) => {
      // do something with data.listingID
    });
    ```

  **[⬆ torna all'inizio](#sommario)**

## jQuery

  <a name="jquery--dollar-prefix"></a><a name="25.1"></a>
  - [26.1](#jquery--dollar-prefix) Prefix jQuery object variables with a `$`.

    ```javascript
    // male
    const sidebar = $('.sidebar');

    // bene
    const $sidebar = $('.sidebar');

    // bene
    const $sidebarBtn = $('.sidebar-btn');
    ```

  <a name="jquery--cache"></a><a name="25.2"></a>
  - [26.2](#jquery--cache) Cache jQuery lookups.

    ```javascript
    // male
    function setSidebar() {
      $('.sidebar').hide();

      // ...

      $('.sidebar').css({
        'background-color': 'pink',
      });
    }

    // bene
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...

      $sidebar.css({
        'background-color': 'pink',
      });
    }
    ```

  <a name="jquery--queries"></a><a name="25.3"></a>
  - [26.3](#jquery--queries) For DOM queries Usare Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](https://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="25.4"></a>
  - [26.4](#jquery--find) Usare `find` with scoped jQuery object queries.

    ```javascript
    // male
    $('ul', '.sidebar').hide();

    // male
    $('.sidebar').find('ul').hide();

    // bene
    $('.sidebar ul').hide();

    // bene
    $('.sidebar > ul').hide();

    // bene
    $sidebar.find('ul').hide();
    ```

**[⬆ torna all'inizio](#sommario)**

## ECMAScript 5 Compatibility

  <a name="es5-compat--kangax"></a><a name="26.1"></a>
  - [27.1](#es5-compat--kangax) Refer to [Kangax](https://twitter.com/kangax/)’s ES5 [compatibility table](https://kangax.github.io/es5-compat-table/).

**[⬆ torna all'inizio](#sommario)**

<a name="ecmascript-6-styles"></a>
## ECMAScript 6+ (ES 2015+) Styles

  <a name="es6-styles"></a><a name="27.1"></a>
  - [28.1](#es6-styles) This is a collection of links to the various ES6+ features.

1. [Arrow Functions](#arrow-functions)
1. [Classes](#classes--constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Exponentiation Operator](#es2016-properties--exponentiation-operator)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

  <a name="tc39-proposals"></a>
  - [28.2](#tc39-proposals) Do not Usare [TC39 proposals](https://github.com/tc39/proposals) that have not reached stage 3.

    > Perche? [They are not finalized](https://tc39.github.io/process-document/), and they are subject to change or to be withdrawn entirely. We want to Usare JavaScript, and proposals are not JavaScript yet.

**[⬆ torna all'inizio](#sommario)**

## Standard Library

  The [Standard Library](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)
  contains utilities that are functionally broken but remain for legacy reasons.

  <a name="standard-library--isnan"></a>
  - [29.1](#standard-library--isnan) Usare `Number.isNaN` instead of global `isNaN`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > Perche? The global `isNaN` coerces non-numbers to numbers, returning true for anything that coerces to NaN.
    > If this behavior is desired, make it explicit.

    ```javascript
    // male
    isNaN('1.2'); // false
    isNaN('1.2.3'); // true

    // bene
    Number.isNaN('1.2.3'); // false
    Number.isNaN(Number('1.2.3')); // true
    ```

  <a name="standard-library--isfinite"></a>
  - [29.2](#standard-library--isfinite) Usare `Number.isFinite` instead of global `isFinite`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > Perche? The global `isFinite` coerces non-numbers to numbers, returning true for anything that coerces to a finite number.
    > If this behavior is desired, make it explicit.

    ```javascript
    // male
    isFinite('2e3'); // true

    // bene
    Number.isFinite('2e3'); // false
    Number.isFinite(parseInt('2e3', 10)); // true
    ```

**[⬆ torna all'inizio](#sommario)**

## Testing

  <a name="testing--yup"></a><a name="28.1"></a>
  - [30.1](#testing--yup) **Yup.**

    ```javascript
    function foo() {
      return true;
    }
    ```

  <a name="testing--for-real"></a><a name="28.2"></a>
  - [30.2](#testing--for-real) **No, but seriously**:
    - Whichever testing framework you use, you should be writing tests!
    - Strive to write many small pure functions, and minimize where mutations occur.
    - Be cautious about stubs and mocks - they can make your tests more brittle.
    - We primarily Usare [`mocha`](https://www.npmjs.com/package/mocha) and [`jest`](https://www.npmjs.com/package/jest) at Airbnb. [`tape`](https://www.npmjs.com/package/tape) is also used occasionally for small, separate modules.
    - 100% test coverage is a good goal to strive for, even if it’s not always practical to reach it.
    - Whenever you fix a bug, _write a regression test_. A bug fixed without a regression test is almost certainly going to break again in the future.

**[⬆ torna all'inizio](#sommario)**

## Performance

  - [On Layout & Web Performance](https://www.kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](https://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](https://jsperf.com/try-catch-in-loop-cost/12)
  - [Bang Function](https://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](https://jsperf.com/jquery-find-vs-context-sel/164)
  - [innerHTML vs textContent for script text](https://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](https://jsperf.com/ya-string-concat/38)
  - [Are JavaScript functions like `map()`, `reduce()`, and `filter()` optimized for traversing arrays?](https://www.quora.com/JavaScript-programming-language-Are-Javascript-functions-like-map-reduce-and-filter-already-optimized-for-traversing-array/answer/Quildreen-Motta)
  - Loading...

**[⬆ torna all'inizio](#sommario)**

## Resources

**Learning ES6+**

  - [Latest ECMA spec](https://tc39.github.io/ecma262/)
  - [ExploringJS](https://exploringjs.com/)
  - [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
  - [Comprehensive Overview of ES6 Features](http://es6-features.org/)

**Read This**

  - [Standard ECMA-262](https://www.ecma-international.org/ecma-262/6.0/index.html)

**Tools**

  - Code Style Linters
    - [ESlint](https://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
    - [JSHint](https://jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
  - Neutrino Preset - [@neutrinojs/airbnb](https://neutrinojs.org/packages/airbnb/)

**Other Style Guides**

  - [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
  - [Google JavaScript Style Guide (Old)](https://google.github.io/styleguide/javascriptguide.xml)
  - [jQuery Core Style Guidelines](https://contribute.jquery.org/style-guide/js/)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)
  - [StandardJS](https://standardjs.com)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/cjohansen/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on GitHub](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](https://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Further Reading**

  - [Understanding JavaScript Closures](https://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](https://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](https://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**Books**

  - [JavaScript: The Good Parts](https://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](https://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](https://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X) - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](https://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](https://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](https://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](https://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](https://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](https://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](https://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](https://www.manning.com/books/third-party-javascript) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](https://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](https://eloquentjavascript.net/) - Marijn Haverbeke
  - [You Don’t Know JS: ES6 & Beyond](https://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson

**Blogs**

  - [JavaScript Weekly](https://javascriptweekly.com/)
  - [JavaScript, JavaScript...](https://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](https://bocoup.com/weblog)
  - [Adequately Good](https://www.adequatelygood.com/)
  - [NCZOnline](https://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](https://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [nettuts](https://code.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Air](https://javascriptair.com/)
  - [JavaScript Jabber](https://devchat.tv/js-jabber/)

**[⬆ torna all'inizio](#sommario)**

## In the Wild

  This is a list of organizations that are using this style guide. Send us a pull request and we'll add you to the list.

  - **123erfasst**: [123erfasst/javascript](https://github.com/123erfasst/javascript)
  - **4Catalyzer**: [4Catalyzer/javascript](https://github.com/4Catalyzer/javascript)
  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **AloPeyk**: [AloPeyk](https://github.com/AloPeyk)
  - **AltSchool**: [AltSchool/javascript](https://github.com/AltSchool/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Ascribe**: [ascribe/javascript](https://github.com/ascribe/javascript)
  - **Avant**: [avantcredit/javascript](https://github.com/avantcredit/javascript)
  - **Axept**: [axept/javascript](https://github.com/axept/javascript)
  - **Billabong**: [billabong/javascript](https://github.com/billabong/javascript)
  - **Bisk**: [bisk](https://github.com/Bisk/)
  - **Bonhomme**: [bonhommeparis/javascript](https://github.com/bonhommeparis/javascript)
  - **Brainshark**: [brainshark/javascript](https://github.com/brainshark/javascript)
  - **CaseNine**: [CaseNine/javascript](https://github.com/CaseNine/javascript)
  - **Cerner**: [Cerner](https://github.com/cerner/)
  - **Chartboost**: [ChartBoost/javascript-style-guide](https://github.com/ChartBoost/javascript-style-guide)
  - **Coeur d'Alene Tribe**: [www.cdatribe-nsn.gov](https://www.cdatribe-nsn.gov)
  - **ComparaOnline**: [comparaonline/javascript](https://github.com/comparaonline/javascript-style-guide)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **DoSomething**: [DoSomething/eslint-config](https://github.com/DoSomething/eslint-config)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Drupal**: [www.drupal.org](https://git.drupalcode.org/project/drupal/blob/8.6.x/core/.eslintrc.json)
  - **Ecosia**: [ecosia/javascript](https://github.com/ecosia/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **Evolution Gaming**: [evolution-gaming/javascript](https://github.com/evolution-gaming/javascript)
  - **EvozonJs**: [evozonjs/javascript](https://github.com/evozonjs/javascript)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia](https://github.com/gawkermedia/)
  - **General Electric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **Generation Tux**: [GenerationTux/javascript](https://github.com/generationtux/styleguide)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **GreenChef**: [greenchef/javascript](https://github.com/greenchef/javascript)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **Grupo-Abraxas**: [Grupo-Abraxas/javascript](https://github.com/Grupo-Abraxas/javascript)
  - **Happeo**: [happeo/javascript](https://github.com/happeo/javascript)
  - **Honey**: [honeyscience/javascript](https://github.com/honeyscience/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript-style-guide)
  - **HubSpot**: [HubSpot/javascript](https://github.com/HubSpot/javascript)
  - **Hyper**: [hyperoslo/javascript-playbook](https://github.com/hyperoslo/javascript-playbook/blob/master/style.md)
  - **InterCity Group**: [intercitygroup/javascript-style-guide](https://github.com/intercitygroup/javascript-style-guide)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **Kaplan Komputing**: [kaplankomputing/javascript](https://github.com/kaplankomputing/javascript)
  - **KickorStick**: [kickorstick](https://github.com/kickorstick/)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/Javascript-style-guide)
  - **LEINWAND**: [LEINWAND/javascript](https://github.com/LEINWAND/javascript)
  - **Lonely Planet**: [lonelyplanet/javascript](https://github.com/lonelyplanet/javascript)
  - **M2GEN**: [M2GEN/javascript](https://github.com/M2GEN/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **MitocGroup**: [MitocGroup/javascript](https://github.com/MitocGroup/javascript)
  - **Muber**: [muber](https://github.com/muber/)
  - **National Geographic**: [natgeo](https://github.com/natgeo/)
  - **NullDev**: [NullDevCo/JavaScript-Styleguide](https://github.com/NullDevCo/JavaScript-Styleguide)
  - **Nulogy**: [nulogy/javascript](https://github.com/nulogy/javascript)
  - **Orange Hill Development**: [orangehill/javascript](https://github.com/orangehill/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **OutBoxSoft**: [OutBoxSoft/javascript](https://github.com/OutBoxSoft/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Pier 1**: [Pier1/javascript](https://github.com/pier1/javascript)
  - **Qotto**: [Qotto/javascript-style-guide](https://github.com/Qotto/javascript-style-guide)
  - **React**: [facebook.github.io/react/contributing/how-to-contribute.html#style-guide](https://facebook.github.io/react/contributing/how-to-contribute.html#style-guide)
  - **REI**: [reidev/js-style-guide](https://github.com/rei/code-style-guides/)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **Sainsbury’s Supermarkets**: [jsainsburyplc](https://github.com/jsainsburyplc)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Sourcetoad**: [sourcetoad/javascript](https://github.com/sourcetoad/javascript)
  - **Springload**: [springload](https://github.com/springload/)
  - **StratoDem Analytics**: [stratodem/javascript](https://github.com/stratodem/javascript)
  - **SteelKiwi Development**: [steelkiwi/javascript](https://github.com/steelkiwi/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/guide-javascript)
  - **SwoopApp**: [swoopapp/javascript](https://github.com/swoopapp/javascript)
  - **SysGarage**: [sysgarage/javascript-style-guide](https://github.com/sysgarage/javascript-style-guide)
  - **Syzygy Warsaw**: [syzygypl/javascript](https://github.com/syzygypl/javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **Terra**: [terra](https://github.com/cerner?utf8=%E2%9C%93&q=terra&type=&language=)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **The Nerdery**: [thenerdery/javascript-standards](https://github.com/thenerdery/javascript-standards)
  - **Tomify**: [tomprats](https://github.com/tomprats)
  - **Traitify**: [traitify/eslint-config-traitify](https://github.com/traitify/eslint-config-traitify)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **UrbanSim**: [urbansim](https://github.com/urbansim/)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **WeBox Studio**: [weboxstudio/javascript](https://github.com/weboxstudio/javascript)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

**[⬆ torna all'inizio](#sommario)**

## Translation

  This style guide is also available in other languages:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [lin-123/javascript](https://github.com/lin-123/javascript)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [ParkSB/javascript-style-guide](https://github.com/ParkSB/javascript-style-guide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [leonidlebedev/javascript-airbnb](https://github.com/leonidlebedev/javascript-airbnb)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)
  - ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [eraycetinay/javascript](https://github.com/eraycetinay/javascript)
  - ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [ivanzusko/javascript](https://github.com/ivanzusko/javascript)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnam**: [dangkyokhoang/javascript-style-guide](https://github.com/dangkyokhoang/javascript-style-guide)

## The JavaScript Style Guide Guide

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## Chat With Us About JavaScript

  - Find us on [gitter](https://gitter.im/airbnb/javascript).

## Contributors

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)

## License

(The MIT License)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE Usare OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ torna all'inizio](#sommario)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team’s style guide. Below, you may list some amendments to the style guide. This allows you to periodically update your style guide without having to deal with merge conflicts.

# };
