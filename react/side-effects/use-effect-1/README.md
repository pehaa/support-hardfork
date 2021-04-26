# Side effects dans React, <code>useEffect()</code>

## Side effects (effets bord)

Pour l'instant nous n'avons pas parler des _side effects_ (en français: effets de bord). Par _side effects_ nous allons comprendre tout ce qui n'est pas directement lié à notre arborescence, mais ce qui se passe en dehors de notre `<div id="root"></div>`.

Prenons quelques examples :

- Nous enregistrons la liste des courses dans une base des données (pour que ce soit efficace, l'enregistrement est effectué à chaque changement de la liste)
- Nous enregistrons la mode couleur choisie dans un système de stockage du navigateur (`localStorage`)
- Nous faisons une requête HTTP
- Nous recevons une réponse (asynchrone) suite à notre requête HTTP
- Nous réagissons aux events qui sont "attachés" en dehors de `<div id="root"></div>`
- Nous utilisons `setTimeout` ou `setInterval`
- Nous modifions le `document` en dehors de notre `<div id="root"></div>`, par exemple nous ,modifion `document.title`

## <code>React.useEffect()</code>

Pour mettre en place des _side effects_, React nous met en disposition un hook `useEffect`.

Pourquoi avons nous besoin d'un hook ? Regardons des exemples suivants 🚫

https://codepen.io/alyra/pen/VwPgXJJ

https://codepen.io/alyra/pen/zYNeaOO

Nous avons besoin d'un mécanisme qui permet de gérer _side effects_ avec plus de contrôle :

- pouvoir executer uniquement quand le components "mounts" (1 fois)
- pouvoir executer uniquement quand une variable change
- pouvoir executer quand le component "unmounts"

## Anatomie du hook `useEffect`

![](https://assets.codepen.io/4515922/useEffectAnatomy.png)

### à chaque render (par défaut)

```javascript
useEffect(() => {
  console.log("je viens de render (ou (re-render))")
})
```

https://codepen.io/alyra/pen/gOrzKyO

### on _mount_, initial render

```javascript
useEffect(() => {
  console.log("I've juste mounted")
}, [])
```

### quand state ou props change

```javascript
useEffect(() => {
  console.log(`myVar1 vient d'être modifiée`, myVariable)
}, [myVariable])
```

Comparons ces 2 exemples :

**Incorrect :** ❌

```javascript
React.useEffect(() => {
  document.title = like ? "👍" : "👎"
})
```

![](https://wptemplates.pehaa.com/assets/alyra/title-ue.gif)

Ici, `title` est re-rendu à chaque re-render de notre component.

---

**Correct :** ✅

```javascript
React.useEffect(() => {
  document.title = like ? "👍" : "👎"
}, [like])
```

![](https://wptemplates.pehaa.com/assets/alyra/title-ue-ok2.gif)

Ici, `title` est re-rendu uniquement si la valeur de la variable `like` change.

https://codepen.io/alyra/pen/VwjPvgG

### Avec cleanup (nettoyage)

```javascript
React.useEffect(() => {
  console.log("mount")
  return () => {
    console.log("cleanup on unmount")
  }
}, [])
```

https://codepen.io/alyra/pen/qBRgYoB

https://codepen.io/alyra/pen/RwKvMrq

## useEffect, localStorage et _lazy initial state_

Nous avons enfin les outils pour mettre en place `localStorage` dans notre applications _Shopping List_.
Nous allons démarrer avec [ce repo](https://github.com/pehaa/alyra-shopping-list-livecoding) et utiliser `useEffect` pour rendre élément `title` dynamique, mais surtout pour enregistrer la liste des courses dans le stockage du navigateur.

### document.title

Nous allons modifier `document.title` en fonction du nombre des produits sur la liste des courses.
Nous allons y mettre soit 'Préparez votre liste des courses' (si elle est vide), soit 'Vous avez .. produit(s) sur votre liste des courses'.

```javascript
// src/components/ShoppingApp.js
import { useState, useEffect } from "react"
/* comme avant */

const ShoppingApp = ({mode}) => {
  const [shopping, setShopping] = useState(["cumin", "curry"])

  /* comme avant */

  useEffect(() => {
    document.title =
      shopping.length === 0
        ? `Préparez vos courses`
        : `${shopping.length} produit(s) sur votre liste des courses`
  }, [shopping])

  return /* comme avant */

export default ShoppingApp
```

Maintenant à chaque fois que `ShoppingApp` _render_ le titre du document est modifié.

---

### localStorage

Nous allons utilisé `localStorage` afin d'enregistrer dans la mémoire du navigateur notre liste des courses et la récupérer à la prochaine visite (après rechargement de la page).

Un petit point sur l'utilisation de `localStorage` :

- `localStorage.setItem("colorMode", mode)` - enregistre la valeur de mode dans l'objet `localStorage`
- `localStorage.getItem("colorMode")` - permet de récupérer la valeur enregistrée sous la clé `"colorMode"`
- localStorage enregistre tout en format de `string`
- afin d'enregistrer un objet, nous utilisons le format JSON :
  - `JSON.stringify(myObjet)` transforme objet `myObjet` en string format JSON
  - `JSON.parse(myJSONString)` transforme `myJSONString` en objet JavaScript

![](https://wptemplates.pehaa.com/assets/alyra/localStorage.png)

Dans notre application, nous avons besoin d'enregistrer la valeur de `shopping` dans `localStorage` à chaque fois que `shopping` change :

```javascript
useEffect(() => {
  localStorage.setItem("myShoppingList", JSON.stringify(shopping))
}, [shopping])
```

Nous allons ensuite besoin de la valeur stockée dans localStorage pour la passer en tant que la valeur initiale de `shopping` :

```javascript
const [shopping, setShopping] = useState( /* ici !! */ )`
```

Alors :

```javascript
const [shopping, setShopping] = useState( JSON.parse(localStorage.getItem('myShoppingList')) || [] )`
```

La valeur initiale de shopping est utilisée uniquement une fois, au moment ou le component monte. Néanmoins, l'expression `JSON.parse(localStorage.getItem('myShoppingList')) || []` sera évaluée à chaque render. Pour y remédier et améliorer la performance (l'échange avec `localStorage` est synchrone), nous allons passer une fonction dans `useState` :

```javascript
const [shopping, setShopping] = useState(
  () => JSON.parse(localStorage.getItem("myShoppingList")) || []
)
```

---

Nous appelons cela _lazy initial state_ ou état local initial paresseux.

Nous allons éviter à utiliser le code suivant :

```javascript
// pas très bien 👎
const [variable, setVariable] = useState(expensiveOperationFunction()) //
```

par contre :

```javascript
//  bien 👍
const [variable, setVariable] = useState(() => expensiveOperationFunction())
```

ou simplement

```javascript
// bien 👍
const [variable, setVariable] = useState(expensiveOperationFunction) //
```

---

## Exercices :

- [Todos App - localStorage et compagnie](https://github.com/pehaa/alyra-todos-localstorage)
