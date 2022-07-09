# Guida di stile Airbnb React/JSX

*Un approccio per lo più ragionevole a React e JSX*

Questa guida di stile si basa principalmente sugli standard che sono attualmente prevalenti in JavaScript, sebbene alcune convenzioni (cioè async/await o campi di classe statica) possano essere ancora inclusi o vietati caso per caso. Attualmente, qualsiasi cosa prima della fase 3 non è incluso né consigliato in questa guida.

## Sommario

  1. [Regole Base](#regole-base)
  1. [Classi vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Mixins](#mixins)
  1. [Denominazione](#denominazione)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Apici](#apici)
  1. [Spaziatura](#spaziatura)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentesi](#parentesi)
  1. [Tag](#tag)
  1. [Metodi](#metodi)
  1. [Ordinamento](#ordinamento)
  1. [`isMounted`](#ismounted)

## Regole Base

  - Includere un solo componente React per file.
    - In ogni caso [Componenti Stateless, o Puri,](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) multipli sono concessi per file. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - Usare sempre la sintassi JSX.
  - Non usare `React.createElement` a meno che tu non si stia inizializzando l'app da un file che non è JSX.
  - [`react/forbid-prop-types`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/forbid-prop-types.md) consente  `array` e oggetti solo se viene esplicitamente annotato cosa è contenuto in `array` e oggetto usando `arrayOf`, `objectOf`, o `shape`.

## Classi vs `React.createClass` vs stateless

  - Se si ha uno stato interno e/o ref, si preferisca `class extends React.Component` a `React.createClass`. eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // male
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // bene
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    Se non si hanno stati o refs, si preferiscano  normali funzioni (non funzioni arrow) alle classes:

    ```jsx
    // male
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // male (appoggiarsi all'inferenza tramite nome di funzione è sconsigliato)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // bene
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Mixins

  - [Non usare mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

  > Perché? I mixins introducono dipendenze implicite, causano confitti di nome, e causano complessità cresente. La maggior parte dei casi di uso per mixin può essere gestita in miglior modo con componenti, componenti di alto livello o moduli di utilità.

## Denominazione

  - **Estensioni**: Usare estensioni `.jsx` per i componenti React. eslint: [`react/jsx-filename-extension`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)
  - **Filename**: Usare PascalCase per i nomi di file. Es., `ReservationCard.jsx`.
  - **Denominazione di Riferimenti**: Usare PascalCase per componenti React e camelCase per le loro istanze. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // male
    import reservationCard from './ReservationCard';

    // bene
    import ReservationCard from './ReservationCard';

    // male
    const ReservationItem = <ReservationCard />;

    // bene
    const reservationItem = <ReservationCard />;
    ```

  - **Denominazione componenti**: Usare il nome del file come nome del componente. Per esempio, `ReservationCard.jsx` dovrebbe avere un nome di riferimento di `ReservationCard`. Tuttavia, per i componenti radice di una directory, usare `index.jsx` come nome di file e usare il nome della directory come nome del componente:

    ```jsx
    // male
    import Footer from './Footer/Footer';

    // male
    import Footer from './Footer/index';

    // bene
    import Footer from './Footer';
    ```

  - **Denominazione di Componenti di più alto livello**: Usare l'unione del componente di più alto livello e il nome del componente ad esso passato come nome da visualizzare (`displayName`) sul componente generato. Per esempio, il componente di più alto livello `withFoo()`, quando riceve un componente `Bar` dovrebbe produrre un componente con  un `displayName` di `withFoo(Bar)`.

    > Perché? Il  `displayName` di un componente potrebbe essere usato degli strumenti dello sviluppatore o in messaggi di errore, e avere un valore che esprime chiaramente questa relazione aiuta le persone a capire cosa sta succedendo.

    ```jsx
    // male
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // bene
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
    ```

  - **Denominazione di Props**: Evitare l'uso di componenti DOM come nomi di prop per diversi scopi.

    > Perchè? Le persone si attendono props come `style` e `className` significhino una cosa specifica. Variare questa API per un sottoinsieme della propria app rende il codice meno leggibile e meno manutenibile, e potrebbe causare bug.

    ```jsx
    // male
    <MyComponent style="fancy" />

    // male
    <MyComponent className="fancy" />

    // bene
    <MyComponent variant="fancy" />
    ```

## Dichiarazione

  - Non usare `displayName` per denominare componenti. Denominare il componente per riferimento.

    ```jsx
    // male
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // bene
    export default class ReservationCard extends React.Component {
    }
    ```

## Allineamento

  - Seguire questi stili di allineamento per la sintassi JSX. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

    ```jsx
    // male
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // bene
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // se le prop stanno in una riga mantenerle nella stessa riga
    <Foo bar="bar" />

    // I figli si intentano normalmente
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>

    // male
    {showButton &&
      <Button />
    }

    // male
    {
      showButton &&
        <Button />
    }

    // bene
    {showButton && (
      <Button />
    )}

    // bene
    {showButton && <Button />}

    // bene
    {someReallyLongConditional
      && anotherLongConditional
      && (
        <Foo
          superLongParam="bar"
          anotherSuperLongParam="baz"
        />
      )
    }

    // bene
    {someConditional ? (
      <Foo />
    ) : (
      <Foo
        superLongParam="bar"
        anotherSuperLongParam="baz"
      />
    )}
    ```

## Apici

  - Usare sempre i doppi apici (`"`) per gli attributi JSX, ma apici singoli (`'`) per tutto l'altro JS. eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

    > Perchè? I normali attributi HTML usano tipicamente apici doppi invece di singoli, quindi gli attributi JSX rispecchiano questa convenzione.

    ```jsx
    // male
    <Foo bar='bar' />

    // bene
    <Foo bar="bar" />

    // male
    <Foo style={{ left: "20px" }} />

    // bene
    <Foo style={{ left: '20px' }} />
    ```

## Spaziatura

  - Includere sempre uno spazio singolo in un tag autochiudente. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

    ```jsx
    // male
    <Foo/>

    // molto male
    <Foo                 />

    // male
    <Foo
     />

    // bene
    <Foo />
    ```

  - Non inserire spazi prima o dopo le parentesi graffe JSX. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // male
    <Foo bar={ baz } />

    // bene
    <Foo bar={baz} />
    ```

## Props

  - Usare sempre camelCase per i nomi di prop, o PascalCase se il valore della prop è un componente React.

    ```jsx
    // male
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // bene
    <Foo
      userName="hello"
      phoneNumber={12345678}
      Component={SomeComponent}
    />
    ```

  - Omettere il valore della prop quando è esplicitamente `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // male
    <Foo
      hidden={true}
    />

    // bene
    <Foo
      hidden
    />

    // bene
    <Foo hidden />
    ```

  - Includere sempre una prop `alt` per i tag `<img>`. Se l'immagine è di presentazione, `alt` potrebbe essere una stringa vuota o `<img>` dovrebbe avere `role="presentation"`. eslint: [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

    ```jsx
    // male
    <img src="hello.jpg" />

    // bene
    <img src="hello.jpg" alt="Me waving hello" />

    // bene
    <img src="hello.jpg" alt="" />

    // bene
    <img src="hello.jpg" role="presentation" />
    ```

  - Non usare parole come "immagine", "foto", o "disegno" nelle prop `<img>` `alt`. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

    > Perchè? I lettori di schermo già annunciano gli elementi `img` come immagini, quindi non serve includere questa informazione nel testo `alt`.

    ```jsx
    // male
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // bene
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Usare solo [ruoli ARIA](https://www.w3.org/TR/wai-aria/#usage_intro) validi e non astratti. eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // male - non un ruolo ARIA
    <div role="datepicker" />

    // male - ruolo ARIA astratto
    <div role="range" />

    // bene
    <div role="button" />
    ```

  - Non usare `accessKey` sugli elementi. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > Perchè? Le inconsistenze tra scorciatoie da tastiera e comandi da tastiera usati dalle persone che utilizzano lettori di schermo e tastiere complicano l'accessibilità.

  ```jsx
  // male
  <div accessKey="h" />

  // bene
  <div />
  ```

  - Evitare l'utilizzo di un indice di array come prop `key`, si preferisca un ID stabile. eslint: [`react/no-array-index-key`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-array-index-key.md)

> Perchè? Non usare un ID stabile [è un anti-modello](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318) in quanto può impattare negativamente le prestazioni e causare problemi con lo stato dei componenti.

L'uso degli indici non è da noi raccomantato per chiavi se l'ordine degli elementi potrebbe cambiare.

  ```jsx
  // male
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // bene
  {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
  ))}
  ```

  - Definire sempre esplicitamente valori predefiniti (defaultProp) per tutte le prop non richieste.

  > Perchè? I propType sono una forma di documentazione, e fornire dei defaultProp significa che chi legge il proprio codice non deve fare troppe assunzioni. Inoltre potrebbe voler dire che il proprio codice possa omettere alcune verifiche di tipo.

  ```jsx
  // male
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };

  // bene
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };
  SFC.defaultProps = {
    bar: '',
    children: null,
  };
  ```

  - Usare la sintassi spread per le prop sporadicamente.
  > Perchè? Altrimenti è più probabile che si passino prop non necessarie verso i componenti. A per React v15.6.1 e meno recenti si potrebbero [passare attributi HTML non validi al DOM](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

  Eccezioni:

  - Componenti di più alto livello (HOC) che fanno da proxy di prop e hanno propType

  ```jsx
  function HOC(WrappedComponent) {
    return class Proxy extends React.Component {
      Proxy.propTypes = {
        text: PropTypes.string,
        isLoading: PropTypes.bool
      };

      render() {
        return <WrappedComponent {...this.props} />
      }
    }
  }
  ```

  - Si usi la sintassi spread per oggetti con prop note ed esplicite. Questo può essere particolarmente utile durante il test di componenti React con il costrutto beforeEach  di  Mocha.

  ```jsx
  export default function Foo {
    const props = {
      text: '',
      isPublished: false
    }

    return (<div {...props} />);
  }
  ```

  Istruzioni per l'uso:
  Escludere prop non necessarie quando possibile. Inoltre usare [prop-types-exact](https://www.npmjs.com/package/prop-types-exact) per aiutare la prevenzione di bug.

  ```jsx
  // male
  render() {
    const { irrelevantProp, ...relevantProps } = this.props;
    return <WrappedComponent {...this.props} />
  }

  // bene
  render() {
    const { irrelevantProp, ...relevantProps } = this.props;
    return <WrappedComponent {...relevantProps} />
  }
  ```

## Ref

  - Usare sempre callback ref. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

    ```jsx
    // male
    <Foo
      ref="myRef"
    />

    // bene
    <Foo
      ref={(ref) => { this.myRef = ref; }}
    />
    ```

## Parentesi

  - Racchiudere i tag JSX tra parentesi quando si sviluppano per più di una riga. eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

    ```jsx
    // male
    render() {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // bene
    render() {
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // bene, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tag

  - Autochiudere sempre i tag che non hanno discendenti. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // male
    <Foo variant="stuff"></Foo>

    // bene
    <Foo variant="stuff" />
    ```

  - Se il proprio componente ha proprierà multiriga, chiudere il suo tag su di una nuova riga. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // male
    <Foo
      bar="bar"
      baz="baz" />

    // bene
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Metodi

  - Usare funzioni arrow per la chiusura su variabili locali. E' conveniente quando serve passare dati aggiuntivi a un gestore di evento. Tuttavia assicurarsi che [non inficino massivamente le prestazioni](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/), in particolare quando passate a componenti personalizzati che potrebbero essere PureComponents, in quanto scateneranno una possibile inutile rirenderizzazione ogni volta.

    ```jsx
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={(event) => { doSomethingWith(event, item.name, index); }}
            />
          ))}
        </ul>
      );
    }
    ```

  - Attaccare gestori di eventi per il metodo di render nel costruttore. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

    > Perchè? A Una chiamata di bind nel percorso di rendereizzazione crea un nuova funzione per ogni singola renderizzazione. Non usare funzioni arrow in campi di classe, in quanto li rende [difficoltosi da testare e per trovare bug e possono impattare negativamente le prestazioni](https://medium.com/@charpeni/arrow-functions-in-class-properties-might-not-be-as-great-as-we-think-3b3551c440b1), e poichè, concettualmente, i campi di classe sono per i dati e non per la logica.

    ```jsx
    // male
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />;
      }
    }

    // molto male
    class extends React.Component {
      onClickDiv = () => {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }

    // bene
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />;
      }
    }
    ```

  - Non usare trattini di sottolineatura per prefissare metodi interni di un componente React.
    > Perchè? I prefissi con trattino di sottolineatura sono talvolta usati come convenzione in altri linguaggi per denotare privacy. Ma, a differenza di altri linguaggi, non c'è supporto nativo per privacy in JavaScript, tutto è pubblico. A prescindere dalle proprie intenzioni, aggiungere trattini di sottolineatura come prefisso per le proprie proprietà non le rende in effetti private, e qualunque proprietà (con prefisso o meno) dovrebbe essere considerata come pubblica. Vedere il problem [#1024](https://github.com/airbnb/javascript/issues/1024), e [#490](https://github.com/airbnb/javascript/issues/490) per una discussione più dettagliata.

    ```jsx
    // male
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // bene
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

  - Assicurarsi che i propri metodi `render` ritornino un valore. eslint: [`react/require-render-return`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)

    ```jsx
    // male
    render() {
      (<div />);
    }

    // bene
    render() {
      return (<div />);
    }
    ```

## Ordinamento

  - Ordinamento per `class extends React.Component`:

  1. metodi `static` opzionali
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *gestori di eventi che iniziano con'handle'* come `handleSubmit()` o `handleChangeDescription()`
  1. *gestori di eventi che iniziano con'on'* come `onClickSubmit()` o `onChangeDescription()`
  1. *metodi getter per `render`* come `getSelectReason()` o `getFooterContent()`
  1. *metodi render opzionali* come `renderNavigation()` o `renderProfilePicture()`
  1. `render`

  - Come definire `propTypes`, `defaultProps`, `contextTypes`, ecc...

    ```jsx
    import React from 'react';
    import PropTypes from 'prop-types';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>;
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - Ordinamento per `React.createClass`: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers o eventHandlers* come `onClickSubmit()` o `onChangeDescription()`
  1. *metodi getter per`render`* come `getSelectReason()` o `getFooterContent()`
  1. *metodi render opzionali * come `renderNavigation()` o `renderProfilePicture()`
  1. `render`

## `isMounted`

  - Non usare `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > Perchè? [`isMounted` è un anti-modello][anti-pattern], non è disponibile quando si usano classi ES6, e sta per essere ufficialmente deprecato.

  [anti-modekki]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

## Traduzioni

  Questa guida di stile JSX/React è anche disponibile in altre lingue:

  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [jhcccc/javascript](https://github.com/jhcccc/javascript/tree/master/react)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript/tree/master/react)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Español**: [agrcrobles/javascript](https://github.com/agrcrobles/javascript/tree/master/react)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide/tree/master/react)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [apple77y/javascript](https://github.com/apple77y/javascript/tree/master/react)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [pietraszekl/javascript](https://github.com/pietraszekl/javascript/tree/master/react)
  - ![Br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese**: [ronal2do/javascript](https://github.com/ronal2do/airbnb-react-styleguide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [leonidlebedev/javascript-airbnb](https://github.com/leonidlebedev/javascript-airbnb/tree/master/react)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide/tree/master/react)
  - ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [alioguzhan/react-style-guide](https://github.com/alioguzhan/react-style-guide)
  - ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [ivanzusko/javascript](https://github.com/ivanzusko/javascript/tree/master/react)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnam**: [uetcodecamp/jsx-style-guide](https://github.com/UETCodeCamp/jsx-style-guide)

**[⬆ torna all'inizio](#table-of-contents)**
