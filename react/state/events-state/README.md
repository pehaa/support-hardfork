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

- Les événements de React sont nommés **en camelCase** :

  - `onClick`,
  - `onChange`,
  - `onSubmit`,
  - `onFocus`,
  - `onBlur`,
  - `onKeyUp`,
  - `onMouseOver`,
  - ...

- En JSX on passe **une fonction** dans _event handler_ (gestionnaire d’événements)

![](https://wptemplates.pehaa.com/assets/alyra/events-react.png)

---

Dans l'exemple suivant nous allons un pas plus loin. La fonction passée dans _event handler_ a accès à l'objet `event`.

```javascript
<Button
  onClick={(event) => {
    console.log(event)
  }}
/>
```

ou

```javascript
const handleButtonClick = (event) => {
  console.log(event)
  /* ... */
}
...
<Button onClick={handleButtonClick} />
```

Ceci dit React a accès aux certaines informations concernant l'évènement.

En particulier, nous pouvons profiter de `event.target` pour avoir l'accès à l'élément qui a déclenché _event_.

Par exemple, nous pouvons lire la valeur du champs `input` avec `event.target.value`.

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

### Component interactif

Dans React, _state_ fait référence de toute variable dont la valeur peut être mis à jour. Quand une des variables de _state_ change, le component React se met à jour : la fonction qui définit le component re-execute, React compare le nouveau _tree_ avec le _tree_ précédent. Il détecte des changements et met à jour le DOM.

Bien évidemment pas tous les components sont interactifs. Les components dont le markup ne change pas et sont appelés _stateless_ (sans _state_).

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

## `React.useState` hook

https://codepen.io/alyra/pen/vYgXPew

```javascript
const [count, setCount] = React.useState(0)
```

et pareil pour `name`

```javascript
const [name, setName] = React.useState("Inconnu")
```

On appelle `React.useState` avec la valeur initiale de la variable state.
`React.useState` **retourne un array,** son premier élément est la valeur en cours de notre variable, la seconde est la fonction pour modifier sa valeur.

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

---

## Exercices :

- [Events - select change](https://codepen.io/alyra/pen/zYqzxgv) | [solution](https://codepen.io/alyra/pen/5a51da89910b5b8aa9e33e55e64450c2)
- [Events - select change](https://codepen.io/alyra/pen/oNxwXWG) | [solution](https://codepen.io/alyra/pen/683f280874ff466121c5115dd0d20943)
- [State - Fix me - click count](https://codepen.io/alyra/pen/jOqYOQN) | [solution](https://codepen.io/alyra/pen/5f1330ce589c02ee762400ee9579e427)
- [Clicks countdown](https://codepen.io/alyra/pen/GRZyWxb) | [solution](https://codepen.io/alyra/pen/19bc3ca167473f4a385088681fcd4011)
- [Clicks compteurs](https://codepen.io/alyra/pen/NWNXper) | [solution](https://codepen.io/alyra/pen/4039761b2dceb4365b22730e69e9de04)
- [State - en lire plus](https://codepen.io/alyra/pen/KKzNoNx) | [solution](https://codepen.io/alyra/pen/ac7586841147fe68381f340d7dc2d46b)
