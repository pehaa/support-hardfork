# Hello React

React est la plus librairie JavaScript. React open-source, créée et maintenue par Facebook, distribuée sous la licence MIT.

**React est une bibliothèque JavaScript pour créer de interfaces utilisateurs (UI).** React affiche des éléments UI ainsi que gére le côté intéractif d'une application.

Depuis quelques années, React est le choix numéro un des développeurs web. Ceci est confirmé par les sondages menés par [Stack Overflow](https://insights.stackoverflow.com/survey/2019/#technology-_-most-loved-dreaded-and-wanted-web-frameworks), [State of Frontend 2020](https://tsh.io/state-of-frontend/#frameworks) ou encore [State of JS](https://2020.stateofjs.com/en-US/technologies/front-end-frameworks/).

De nombreuses grandes entreprises utilisent React en production, parmi elles bien évidemment Facebook (et Instagram), mais aussi Netflix, Airbnb, Cloudflare ou Dropbox.

## Approche _native_, DOM Web API

Avant d'utiliser React, nous allons intéragir avec la page web avec JavaScript et l'API DOM

```js
const element = document.createElement("h1")
element.textContent = "Nos premiers pas avec l'API DOM"
element.className = "text-center text-uppercase"
document.body.append(element)
```

```js
const container = document.querySelector("#some-id")
// const container = document.getElementById("some-id")
container.append(element)
```

### Exercices DOM

- [Veilles intro](https://codepen.io/alyra/pen/jOVgRmz)
- [Veilles intro 2](https://codepen.io/alyra/pen/zYogXdX)
- [Top 5](https://codepen.io/alyra/pen/LYbwvaN)

---

- [Veilles](https://codepen.io/alyra/pen/BaQXELR)
- [Veilles 2](https://codepen.io/alyra/pen/JjbgVbQ)

## React et DOM

Nous allons faire nos premiers pas avec React dans CodePen. Afin de commencer à travailler, nous avons besoin de 2 fichiers sources :

- **React** [https://unpkg.com/react/umd/react.development.js](https://unpkg.com/react/umd/react.development.js)
- **ReactDOM** [https://unpkg.com/react-dom/umd/react-dom.development.js](https://unpkg.com/react-dom/umd/react-dom.development.js)

Avec ces 2 ressources chargées dans la page, nous obtenons deux variables globales, des objets `React` et `ReactDOM`.

Pendant que <b>`React`</b> crée une représentation virtuelle de l'interface utilisateur (appelé DOM virtuel), <b>`ReactDOM`</b> met à jour efficacement le DOM en fonction de ce DOM virtuel. Autrement dit, `ReactDOM` permet de lier React et le DOM.

## React.createElement + ReactDOM.render

Avec les objets `React` et `ReactDOM` disponibles dans la page, nous allons commencer par les méthodes `React.createElement` et `ReactDOM.render`.

Pour mieux comprendre le fonctionnement de React, et en particulier ces méthodes, nous allons procéder par la comparaison de deux approches :

- création d'interface utilisateur avec DOM
- création d'interface utilisateur avec React et ReactDOM

```html
<!-- Approche Native -->
<div class="container" id="root"></div>
<script>
  const rootElement = document.getElementById("root-dom")
  // type d'élément
  const paragraphEl = document.createElement("p")
  // contenu textuel
  paragraphEl.textContent = "Hello World"
  // ajouter des classes
  paragraphEl.className = "bg-danger text-white"
  rootElement.append(paragraphEl)
</script>
```

```html
<!-- Approche React -->
<div class="container" id="root"></div>
<script src="https://unpkg.com/react/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom/umd/react-dom.development.js"></script>
<script>
  const rootElement = document.getElementById("root")
  const paragraphEl = React.createElement(
    "p", // type d'élément
    {
      className: "bg-danger text-white", // ajouter des classes
    },
    "Hello World" // contenu textuel
  )
  ReactDOM.render(paragraphEl, rootElement)
</script>
```

```html
<!-- Approche Native -->
<div class="container" id="root"></div>
<script>
  const rootElement = document.getElementById("root-dom")
  const paragraphEl = document.createElement("p")
  paragraphEl.textContent = "Hello World"
  paragraphEl.className = "bg-danger text-white"
  rootElement.append(paragraphEl)
</script>
```

```html
<!-- Approche React -->
<div class="container" id="root"></div>
<script src="https://unpkg.com/react/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom/umd/react-dom.development.js"></script>
<script>
  const rootElement = document.getElementById("root")
  const paragraphEl = React.createElement(
    "p",
    {
      className: "bg-danger text-white",
    },
    "Hello World"
  )
  ReactDOM.render(paragraphEl, rootElement)
</script>
```

https://codepen.io/alyra/pen/PoNGGJM

Pour résumer, voici comment créer et insérer un élément React :

```javascript
// créer un élément
const element = React.createElement(
  type, // "p" | "div" | "img" | etc.
  [props], // null | {} | {id: "top"} | {id: "top", className: "text-center bg-danger"}
  [...children] // "Hello !" | "Hello", " Ravis de vous revoir!"
)
// l'insérer/mettre à jour dans domCointainer
ReactDOM.render(
  element, // reactElement
  domContainer // DOM Element
)
```

## Exemples

```javascript
const element = React.createElement("p")
ReactDOM.render(element, document.getElementById("root"))
// <p></p>
```

---

```javascript
const element = React.createElement("input")
ReactDOM.render(element, document.getElementById("root"))
// <input/>
```

---

```javascript
const element = React.createElement(
  "h1",
  null, // on met null ou {} si nous n'avons pas de props à passer
  "Je suis le titre"
)
ReactDOM.render(element, document.getElementById("root"))
// <h1>Je suis le titre</h1>
```

L'exemple précédent peut être réécrit comme ceci :

```javascript
const element = React.createElement("h1", {
  children: "Je suis le titre",
})
ReactDOM.render(element, document.getElementById("root"))
// <h1>Je suis le titre</h1>
```

---

```javascript
const element = React.createElement(
  "h1",
  {
    className: "display-1",
    lang: "en",
    id: "top",
  },
  "Hello World!"
)
ReactDOM.render(element, document.getElementById("root"))
// <h1 class="display-1" lang="en" id="top">Hello World!</h1>
```

ou :

```javascript
const element = React.createElement("h1", {
  className: "display-1",
  lang: "en",
  id: "top",
  children: "Hello World!",
})
ReactDOM.render(element, document.getElementById("root"))
// <h1 class="display-1" lang="en" id="top">Hello World!</h1>
```

---

```javascript
const element = React.createElement(
  "p",
  {
    className: "lead",
  },
  "Bonjour le Monde !",
  " Notre aventure avec React commence."
)
ReactDOM.render(element, document.getElementById("root"))
// <p class="lead">Bonjour le Monde ! Notre aventure avec React commence.</p>
```

ou :

```javascript
const element = React.createElement("p", {
  className: "lead",
  children: ["Bonjour le Monde !", " Notre aventure avec React commence."],
})
ReactDOM.render(element, document.getElementById("root"))
// <p class="lead">Bonjour le Monde ! Notre aventure avec React commence.</p>
```

---

```javascript
const element = React.createElement(
  "article",
  null,
  React.createElement("h2", null, "Secrets des pandas roux"),
  React.createElement("img", {
    src: "panda-roux.png",
    alt: "Panda roux sur une branche.",
  }),
  React.createElement(
    "p",
    null,
    "Le saviez-vous ? Le petit panda est un carnivore, qui ne mange jamais de la viande."
  )
)
ReactDOM.render(element, document.getElementById("root"))
/*
<article class="card">
  <h2>Secrets des pandas roux</h2>
  <img src="panda-roux.png" alt="Panda roux sur une branche.">
  <p>Le saviez-vous ? Le petit panda est un carnivore, qui ne mange jamais de la viande.</p>
</article>
*/
```

https://codepen.io/alyra/pen/xxVLwgY

## React.Fragment

Dans tous les exemples ci-dessus, nous avons **un seul** élément parent qui a un ou plusieurs noeuds enfants. Afin d'avoir des "siblings" (👭) nous devons utiliser `React.Fragment`, un conteneur sans type ni attribut, un conteneur fantôme 👻.

```javascript
const element = React.createElement(
  React.Fragment,
  null,
  React.createElement("h1", { lang: "en" }, "Hello World!"),
  React.createElement(
    "p",
    { className: "subtitle" },
    "Les premiers mots d'un logiciel"
  )
)
/*
<h1 lang="en">Hello World!</h1>
<p class="subtitle">Les premiers mots d'un logiciel</p>
*/
```

Ceci peut être réécrit comme ci-dessous, en affectant des éléments React aux variables :

```javascript
const h1 = React.createElement("h1", { lang: "en" }, "Hello World!")
const p = React.createElement(
  "p",
  { className: "subtitle" },
  "Les premiers mots d'un logiciel"
)
const element = React.createElement(React.Fragment, null, h1, p)
/*
// ou comme ceci :
const element = React.createElement(
  React.Fragment,
  {
    children: [h1, p]
  }
)
*/

/*
<h1 lang="en">Hello World!</h1>
<p class="subtitle">Les premiers mots d'un logiciel</p>
*/
```

**Petite digression :** En fait, [_fragment_ existe aussi dans le DOM.](https://developer.mozilla.org/fr/docs/Web/API/DocumentFragment) Voici un exemple de son utilité :

```javascript
// cryptoCurrencies - un array de 5000 éléments
const ul = document.getElementById("my-crypto-currencies")
const fragment = document.createDocumentFragment()
for (let crypto of cryptoCurrencies) {
  const li = document.createElement("li")
  li.textContent = crypto
  // ul.append(li) <- provoque reflow
  fragment.append(li) // pas de reflow
}

ul.append(fragment) // reflow, tous les nouveaux noeuds sont ajoutés une seule fois (👍)
```

Dans l'exemple ci-dessus, grâce au _fragment_, on a économisé l'utilisation de la méthode `append` qui provoque l'opération de _reflow_ dans le navigateur. À chaque _reflow_, le navigateur recalcule et re-dessine la page. Avec _fragment_, nous allons provoquer un seul _reflow_, au lieu de 5000 de _reflows_ potentiels.

## Les attributs

Dans la majorité des cas, on utilise des attributs de la même façon que dans HTML. Il y a pourtant quelques exceptions, voici quelques exemples :

- on n'utilise pas l'attribut HTML `class` mais `className`

```javascript
const element = React.createElement(
  "h1",
  {
    className: "display-1 text-center",
  },
  "Bienvenue et bonne journée !!"
)
```

- on n'utilise pas l'attribut HTML `for` (`label`) mais `htmlFor`

```javascript
React.createElement(
  'label',
  {
    htmlFor: "mail"
  },
  "Votre adresse e-mail"
}
```

- la valeur de l'attribut `style` n'est plus un string mais un object

```javascript
const element = React.createElement(
  "h1",
  {
    style: {
      letterSpacing: "2px",
      color: "magenta",
    },
  },
  "Bienvenue et bonne journée !!"
)
```

Faites attention aux noms des propriétés en camelCase et des valeurs avec des guillemets (!) Sachez aussi que la directive `!important` ne marche pas avec le style inline dans React.

---

## Exercices :

- [React.createElement - p](https://codepen.io/alyra/pen/mdProPw) | [solution](https://codepen.io/alyra/pen/996c540d3b4910dd8e7f44a09bd4bbc9)
- [React.createElement - button](https://codepen.io/alyra/pen/jOqMJZJ) | [solution](https://codepen.io/alyra/pen/c2fbab1869a4e5a3e42180cf8ac203ef)
- [React.createElement - h1](https://codepen.io/alyra/pen/rNeMRRx) | [solution](https://codepen.io/alyra/pen/269e8a2880caa3e09477d603577244c7)
- [React.createElement - input](https://codepen.io/alyra/pen/RwagEMo) | [solution](https://codepen.io/alyra/pen/a00f3fe1653bcff33b3e57a5bd1e1c66)
- [React.createElement - header](https://codepen.io/alyra/pen/RwagEBK) | [solution](https://codepen.io/alyra/pen/25ecf36755944b625fcf2dd9a5f715ac)
- [React.createElement - label + input](https://codepen.io/alyra/pen/yLOXZJd) | [solution](https://codepen.io/alyra/pen/6d2563311c69909c16d7cfb471ab7f7a)
- [React.createElement - style](https://codepen.io/alyra/pen/JjXJqPG) | [solution](https://codepen.io/alyra/pen/848f5e1deae270f6ba3c60cf71546f3a)
