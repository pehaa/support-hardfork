# Handling Events & 💫 State 💫

## Handling Events

Dans cette partie nous allons apprendre comment gérer des _events_ (tels que _click_, _change_ ou _submit_) avec React et JSX. Nous allons ainsi rendre nos éléments interactifs.

Voici notre premier exemple, un component `Button` qui affiche un élément `button` avec le texte "Click me". Suite à un `click`, le navigateur envoie une fenêtre alert avec le texte "Hello".

```javascript
const Button = () => {
  return (
    <button type="button" onClick={() => alert("Hello")}>
      Click me
    </button>
  )
}
```

---

Comme vous pouvez l'observer et ce qui sera **à retenir** :

- Les événements de React sont nommés **en camelCase** : `onClick`, `onChange`, `onSubmit`, `onKeyUp`,...
- En JSX on passe **une fonction** dans _event handler_ (gestionnaire d’événements)

![](https://wptemplates.pehaa.com/assets/alyra/events-react.png)

---

Dans l'exemple suivant nous allons un pas plus loin. La fonction passée dans _event handler_ a par défaut accès à l'objet `event`.

```javascript
<Button onClick={(event) => {...}} />
```

ou

```javascript
const handleButtonClick = (event) => {
  ...
}
<Button onClick={handleButtonClick} />
```

En particulier, nous pouvons profiter de `event.target` pour avoir l'accès à l'élément qui a déclenché _event_. Nous pouvons lire la valeur du champs `input` avec `event.target.value`.

```javascript
const Input = () => {
  const handleNameChange = (event) => {
    console.log(`Hello ${event.target.value}`)
  }
  return <input onChange={handleNameChange} placeholder="Votre nom" />
}
```

Je vous propose de suivre une convention pour de noms de fonction dans _event handler._ Nous allons les composer comme ceci :

`handle` + nom d'élément + nom d'_event_

Par exemple :

```javascript
onClick = { handleButtonClick }
```

```javascript
onClick = { handleResetClick }
```

```javascript
onChange = { handleEmailChange }
```

```javascript
onSubmit = { handleLoginSubmit }
```

```javascript
// si on a besoin de passer un argument (ex. id) dans event handler
onClick={(event) => handleButtonClick(id, event)}
```

https://codepen.io/alyra/pen/ZEWvjQN

## State

### Component interactif (approche "from scratch")

Dans React, _state_ variable est une variable qui est responsable pour éventuel re-render d'un component. Si une des variables de _state_ change, le component React se met à jour. Bien évidemment pas tous les components sont interactifs. Les components dont le markup ne change pas et sont appelés _stateless_ (sans _state_).

Voici un exemple de component _stateless_ `<Article>` :

```javascript
const Article = ({ children, title }) => {
  return (
    <article class="shadow mb-4 p-3">
      <h2>{title}</h2>
      <p>{children}</p>
    </article>
  )
}
```

https://codepen.io/alyra/pen/WNxoXWw

Et voici sa "version" interactive :

https://wptemplates.pehaa.com/assets/alyra/state-article.mp4

### Faking "useState"

L'exemple ci-dessous a pour but illustrer comment l'interactivité fonctionne sous le capot :

```javascript
// notre component App a 2 state variables like et name
const state = {
  like: false,
  name: "Inconnu",
}

const setLike = (value) => {
  if (state.like !== value) {
    state.like = value
    renderFunction()
  }
}

const setName = (value) => {
  if (state.name !== value) {
    state.name = value
    renderFunction()
  }
}

const App = () => {
  const { like, name } = state

  const handleLikeChange = () => {
    setLike(event.target.checked)
  }

  const handleNameChange = (event) => {
    setName(event.target.value)
  }

  return (
    <div>
      <input onChange={handleNameChange} />
      <input type="checkbox" onChange={handleLikeChange} />
      <p>
        {name} dit : "{like ? `j'aime React` : `je n'aime pas React`}".
      </p>
    </div>
  )
}
// nous définissons une fonction pour effectuer render
function renderFunction() {
  ReactDOM.render(<App />, document.getElementById("root"))
}
// render initial
renderFunction()
```

https://codepen.io/alyra/pen/VwamzdG

Pour la première fois, nous avons un component interactif.
Nous allons maintenant utiliser la fonction React `useState`, qui prendra en charge la gestion du re-render.

## `React.useState` hook

Afin de mettre en place un component interactif nous devons nous poser la question suivante :

> Quelle est la variable qui potentiellement change la valeur et ce changement devrait déclencher re-render ?

Dans notre cas ce sont `name` et `like`.

Nous allons déclarer `name` et `like` en tant que variables de _state_. React nous donne en disposition un mécanisme qui permet de lier la mise à jour de la valeur d'une variable et la mise de l'affichage du component sur la page (re-render)

Au lieu de déclarer la variable `like` de la façon normale `const like = false`
nous allons faire :

```javascript
const [like, setLike] = React.useState(false)
```

et pareil pour `name`

```javascript
const [name, setName] = React.useState("Inconnu")
```

On appelle `React.useState` avec la valeur initiale de la variable state.
`React.useState` **retourne un array,** son premier élément est une clé de state variable, la seconde est la fonction pour modifier la valeur de notre state variable (setter).

```javascript
const App = () => {
  const [like, setLike] = React.useState(false)
  const [name, setName] = React.useState("Inconnu")

  const handleLikeChange = (event) => {
    setLike(event.target.checked)
  }

  const handleNameChange = (event) => {
    setName(event.target.value)
  }

  return (
    <div>
      <input onChange={handleNameChange} />
      <input type="checkbox" onChange={handleLikeChange} />
      <p>
        {name} dit : "{like ? `j'aime React` : `je n'aime pas React`}".
      </p>
    </div>
  )
}
// nous nous préoccupons uniquement de render initial
ReactDOM.render(<App />, document.getElementById("root"))
```

https://codepen.io/alyra/pen/xxVRPYw

### Simple Counter

https://codepen.io/alyra/pen/NWNXBdL

### Alyra Gradients - header interactif

https://codepen.io/alyra/pen/rNepaOy

## React Hooks et ses règles 

`useState` est un des hooks de React. Nous appelons hooks dans les components React (ou dans les hooks personnalisés). Nous allons bientôt découvrir d'autres hooks (`useContext`, `useEffect`). Les hooks sont des fonctions JavaScript, mais des fonctions un peu spéciales. Ceci dit nous devons suivre certaines règles d'utilisation.

Pour assurer leur fonctionnement correct, hooks doivent être appelés dans le même ordre à chaque affichage du composant. Par conséquent, hooks ne peuvent pas être utilisés :
 - à l’intérieur de boucles
 - à l’intérieur de code conditionnel
 - à l’intérieur de fonctions imbriquées. 
 
 Les Hooks devraient être utilisés au niveau racine d'un component React.
 

## Class Components

React hooks, en particulier `useState` ne marchent pas dans les components qui utilisent la syntaxe de `class`.

Dans ce cas là, nous initialisons `state` dans le constructeur, et utilisant fonction `setState` pour les mises à jour de state.

```javascript
class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      like: true,
      name: "Inconnu",
    }
  }
  handleLikeChange(event) {
    this.setState({ like: event.target.checked })
  }
  handleNameChange(event) {
    this.setState({ name: event.target.value })
  }
  render() {
    return (
      <div>
        <input onChange={() => this.handleNameChange()} />
        <input
          type="checkbox"
          onChange={(event) => this.handleLikeChange(event)}
        />
        <p>
          {this.state.name} dit : "
          {this.state.like ? `j'aime React` : `je n'aime pas React`}".
        </p>
      </div>
    )
  }
}
```

https://codepen.io/alyra/pen/ZEWBvEG

---

## Exercices :

- [Events - select change](https://codepen.io/alyra/pen/zYqzxgv) | [solution](https://codepen.io/alyra/pen/5a51da89910b5b8aa9e33e55e64450c2)
- [Events - select change](https://codepen.io/alyra/pen/oNxwXWG) | [solution](https://codepen.io/alyra/pen/683f280874ff466121c5115dd0d20943)
- [State - Fix me - click count](https://codepen.io/alyra/pen/jOqYOQN) | [solution](https://codepen.io/alyra/pen/5f1330ce589c02ee762400ee9579e427)
- [Clicks countdown](https://codepen.io/alyra/pen/GRZyWxb) | [solution](https://codepen.io/alyra/pen/19bc3ca167473f4a385088681fcd4011)
- [Clicks compteurs](https://codepen.io/alyra/pen/NWNXper) | [solution](https://codepen.io/alyra/pen/4039761b2dceb4365b22730e69e9de04)
- [Dark Mode](https://codepen.io/alyra/pen/OJNzPgL) | [solution](https://codepen.io/alyra/pen/4cab28e7eab6f5a480df16bb73f12e95)
- [State - en lire plus](https://codepen.io/alyra/pen/KKzNoNx) | [solution](https://codepen.io/alyra/pen/ac7586841147fe68381f340d7dc2d46b)
