# Rendering HTML from string dans React

Souvent le contenu que nous affichons avec React provient d'une source tierce (api, base de données). Nous les récupérons en tant qu'objet, par exemple :

```javascript
const alyra = {
  name: "Alyra",
  link: "https://alyra.fr",
  description: `<p>Une école <strong>au coeur de la blockchain.</strong> Fondée par des passionnés et ouverte à toutes et tous.</p>`,
}
```

Dans ce cas-là, `name`, `link`, `description` sont en format `string`. Les clés `description` contiennent les balises HTML. Insérer des balises HTML d'une source tierce est une opération à risque, exposant notre application aux attaques XSS. Pour nous protéger contre ceci, React échappe toutes les balises HTML. Regardons ce que ça veut dire en pratique.

```javascript
const School = (props) => {
  const { name, children } = props
  return (
    <article className="p-3 mb-3 border shadow">
      <h2 className="text-center">{name}</h2>
      {children}
    </article>
  )
}

const App = () => {
  return <School name={alyra.name}>{alyra.description}</School>
}

ReactDOM.render(<App />, document.getElementById("root"))
```

![](https://assets.codepen.io/4515922/alyrahtmlstring.png)

Voici notre **problème** : Description est récupérée depuis le string dans `alyra.description`. Pour des raisons de sécurité, toutes les balises sont converties en string.

```javascript
const htmlString = `<h1>Je suis un string <b>avec HTML !</b></h1>`
const App = () => <div>{htmlString}</div>
```

![](https://assets.codepen.io/4515922/htmlstring.png)

## Solution (1) - dangerouslySetInnerHTML

Afin de rendre le string en format HTML, nous pouvons le passer en tant que props avec `dangerouslySetInnerHTML` comme ci-dessus :

```javascript
const htmlString = `<h1>Je suis un string <b>avec HTML !</b></h1>`

const App = () => <div dangerouslySetInnerHTML={{ __html: htmlString }} />
```

`dangerouslySetInnerHTML` est une props React, que nous pouvons utiliser avec des balises de DOM (mais pas avec des components ou `React.Fragment`).  
La valeur de `dangerouslySetInnerHTML` doit être un objet avec une clé `__html`.

Nous pouvons comparer `dangerouslySetInnerHTML` à `innerHTML` dans le DOM.

Voici comment nous utilisons `dangerouslySetInnerHTML` dans notre `<School />` component :

```javascript
const alyra = {
  name: "Alyra",
  description: `<p>Une école <strong>au coeur de la blockchain.</strong> Fondée par des passionnés et ouverte à toutes et tous.</p>`,
}

const School = (props) => {
  const { name, children } = props
  return (
    <article className="p-3 mb-3 border shadow">
      <h2 className="text-center">{name}</h2>
      <div dangerouslySetInnerHTML={{ __html: children }} />
    </article>
  )
}

const App = () => {
  return <School name={alyra.name}>{alyra.description}</School>
}

ReactDOM.render(<App />, document.getElementById("root"))
```

https://codepen.io/alyra/pen/WNwXNmv

## Solution (1a) - dangerouslySetInnerHTML + DOMPurify

[DOMPurify](https://github.com/cure53/DOMPurify) est une bibliothèque JavaScript, qui permet de nettoyer le HTML en enlevant uniquement les parties potentiellement dangereuses.

```javascript
const dirty = "<img src=x onerror=alert(1)//>"
const clean = DOMPurify.sanitize(dirty)
// clean <img src="x">
```

https://codepen.io/alyra/pen/ZEWjbQy

## Solution (2) html-react-parser

`dangerouslySetInnerHTML` n'est pas la seule solution. Il est aussi possible d'utiliser une bibliothèque externe, telle que par exemple [`html-react-parser.`](https://github.com/remarkablemark/html-react-parser)

`html-react-parser` est un outil puisant, mais sa fonctionnalité principale est de transformer un HTML string en objet React.

```html
<div id="root"></div>
<script src="https://unpkg.com/react/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/html-react-parser@latest/dist/html-react-parser.min.js"></script>
```

Voici comment nous pouvons l'utiliser avec notre `<School />` exemple :

```javascript
const alyra = {
  name: "Alyra",
  description: `<p>Une école <strong>au coeur de la blockchain.</strong> Fondée par des passionnés et ouverte à toutes et tous.</p>`,
}

const School = (props) => {
  const { name, children } = props
  return (
    <article className="p-3 mb-3 border shadow">
      <h2 className="text-center">{name}</h2>
      {children}
    </article>
  )
}

const App = () => {
  return <School name={alyra.name}>{HTMLReactParser(alyra.description)}</School>
}

ReactDOM.render(<App />, document.getElementById("root"))
```

https://codepen.io/alyra/pen/dyMZPdE

## Solution (2a) html-react-parser + DOMPurify

https://codepen.io/alyra/pen/gOrjaMY

```html
<div id="root"></div>
<script src="https://unpkg.com/react/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/html-react-parser@latest/dist/html-react-parser.min.js"></script>
<script
  src="https://cdnjs.cloudflare.com/ajax/libs/dompurify/2.0.15/purify.min.js"
  integrity="sha512-nLBKiXCB1MA5X1IwvJ/HLy7fq5XFZlWUlwvCpDJE2f661evdFXQxt1Zk2NBQyveeESfV566CEc8t0SuFrwqxSA=="
  crossorigin="anonymous"
></script>
```

```javascript
const alyra = {
  name: "Alyra",
  description: `<p>Une école <strong>au coeur de la blockchain.</strong> Fondée par des passionnés et ouverte à toutes et tous.</p>`,
}

const School = (props) => {
  const { name, children } = props
  return (
    <article className="p-3 mb-3 border shadow">
      <h2 className="text-center">{name}</h2>
      {children}
    </article>
  )
}

const App = () => {
  return (
    <School name={alyra.name}>
      {HTMLReactParser(DOMPurify.sanitize(alyra.description))}
    </School>
  )
}

ReactDOM.render(<App />, document.getElementById("root"))
```
