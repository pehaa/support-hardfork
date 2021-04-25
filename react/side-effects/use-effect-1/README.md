# Side effects dans React & <code>useEffect()</code> hook

## Side effects (effets bord)

L'application React "vit" dans un élément de la page `<div id="root"></div>`. React crée une arborescence des éléments qu'il gère et injecte un markup dans root. Le markup peut être mis à jour si une variable de state change sa valeur. Dans ce cas la une partie de l'arborescence de React est re-evaluée.

Pour l'instant nous n'avons pas parler des _side effects_ (en français: effets de bord). Par _side effects_ nous allons comprendre tout ce qui n'est pas directement lié à notre arborescence, tout ce qui se passe en dehors de notre `<div id="root"></div>`.

Prenons quelques examples :

- Nous enregistrons la liste des courses dans une bonne des données (pour que ce soit efficace, l'enregistrement est effectué à chaque changement de la liste)
- Nous enregistrons la mode couleur choisie dans un système de stockage du navigateur (`localStorage`)
- Nous faisons une requette HTTP
- Nous récévons une réponse (asynchrone) suite à notre requette HTTP
- Nous réagissone aux events qui sont "attachés" en dehors de `<div id="root"></div>`, en particulier les events qui sont attaché à l'objet `window` (par exemple scroll)
- Nous utilisons `setTimeout` ou `setInterval`
- Nous modifions le `document` en dehors de notre component ou même toute notre application (en dehors de notre `<div id="root"></div>`), par exemple `document.title`

## <code>React.useEffect()</code>

Pour mettre en place des _side effects_, React nous met en disposition un hook `useEffect`.

Pourquoi avons nous besoin d'un hook ? Regardons l'exemple suivant

https://codepen.io/alyra/pen/RwKvMrq

Nous avons besoin d'un méchanisme qui permet de gérer _side effects_ avec plus de contrôle :

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

### au montage (_mount_, initial render)

```javascript
useEffect(() => {
  console.log("je viens de monter!!!")
}, [])
```

Ex. 1

https://codepen.io/alyra/pen/PoNeaMq

Ex. 2

https://codepen.io/alyra/pen/jOqxKjB

### quand state ou props change

```javascript
useEffect(() => {
  console.log(`myVar1 vient d'être modifiée`, myVar1)
}, [myVar1])
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

### avec nettoyage

```javascript
React.useEffect(() => {
  console.log("mount")
  return () => {
    console.log("cleanup on unmount")
  }
}, [])
```

```javascript
React.useEffect(() => {
  console.log("render")
  return () => {
    console.log("cleanup before re-render")
  }
})
```

https://codepen.io/alyra/pen/BaKxxpx

## useEffect, localStorage et _lazy initial state_

Nous avons enfin les outils pour mettre en place `localStorage` dans nos applications _Shopping List_ et _ToDo List._
Nous allons démarrer avec [ce repo](https://github.com/pehaa/alyra-shopping-list-useeffect) et utiliser `useEffect` pour rendre élément `title` dynamique, mais surtout pour enregistrer la liste des courses et le thème choisi par l'utilisateur dans le navigateur.

### document.title

Nous allons modifier `document.title` en fonction du nombre des produits sur la liste des courses.
Nous allons y mettre soit 'Préparez votre liste des courses' (si elle est vide), soit 'Vous avez .. produit(s) sur votre liste des courses'.

L'élément `title` se trouve dans la partie `head` de notre document HTML. C'est en dehors du _scope_ de notre application. Le titre devrait se mettre à jour à chaque fois où le nombre de produits sur la liste change. Nous allons dons utiliser le hook `useEffect` avec le deuxième paramètre `[shopping.length]`.

```javascript
// src/components/ShoppingApp.js
import React, { useState, useEffect } from "react"
/* comme avant */

const ShoppingApp = ({mode}) => {
  const [shopping, setShopping] = useState(["cumin", "curry"])

  /* comme avant */

  useEffect(() => {
    document.title =
      shopping.length === 0
        ? `Préparez vos courses`
        : `${shopping.length} produit(s) sur votre liste des courses`
  }, [shopping.length])

  return /* comme avant */

export default ShoppingApp
```

Maintenant à chaque fois que `ShoppingApp` _render_ le titre du document est modifié

---

### localStorage

Nous avons utilisé `localStorage` afin d'enregistrer dans la mémoire du navigateur notre liste des courses et la récupérer à la prochaine visite (après rechargement de la page).

Un petit rappel sur l'utilisation de `localStorage` :

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

La valeur initiale de shopping est utilisée uniquement une fois, au moment ou le component monte. Néanmoins, l'expression `JSON.parse(localStorage.getItem('myShoppingList')) || []` sera évaluée à chaque render. Pour y remédier et améliorer la performance (l'échange avec `localStorage` peuvent être coûteuse au niveau de la performance), nous allons passer une fonction dans `useState` :

```javascript
const [shopping, setShopping] = useState(
  () => JSON.parse(localStorage.getItem("myShoppingList")) || []
)
```

---

Nous appelons cela _lazy initial state_ ou état local initial paresseux.

Nous allons éviter à utiliser le code suivant :

```javascript
const [variable, setVariable] = useState(expensiveOperationFunction()) // pas bien 👎
```

par contre :

```javascript
const [variable, setVariable] = useState(() => expensiveOperationFunction()) //  bien 👍
```

ou simplement

```javascript
const [variable, setVariable] = useState(expensiveOperationFunction) //  bien 👍
```

---

## Exercices :

- Utiliser la même approche et modifier le fichier `src/context/ModeContext.js` afin de profiter de localStorage pour stocker la valeur de `mode`.
- [Todos App - localStorage et compagnie](https://github.com/pehaa/alyra-todos-localstorage)
