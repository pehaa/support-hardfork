# Projet Gradients - l'aventure continue

Dans ce challenge vous aller faire un refactoring du projet gradient :

- les gradients sont récupérés via une API `https://gradients-api.herokuapp.com`
- vous utilisez `useReducer` et context API pour gradients
- l'application utilise `react-router-dom`

[Voici la demo](https://alyra-gradients-once-more.netlify.app/)

## Routes avec URL params

Dans ce projet vous devez utiliser ✨ la magie ✨ des routes avec URL parameters

Voici un exemple :

```js
<Switch>
  <Route exact path="/">
    <Home />
  </Route>
  <Route exact path="/product/:id">
    <Product />
  </Route>
</Switch>
```

Le component `Product` _sait_ quelle est la valeur de `id` grâce au hook de `react-router-dom` `useParams` :

```js
// Product.js
import { useParams } from "react-router-dom"

function Product() {
  const params = useParams()
  console.log(params)
  const { id } = params
  return (
    <>
      <h2>Product with id: {id}</h2>
      {/* et tout ce qui concerne un produit avec mon id vient ici !! */}
    </>
  )
}
```

---

Dans notre cas les routes à mettre en place sont :

- `/`
- `/gradient/:id`

Voici des exemples :

- [/gradient/3](https://alyra-gradients-once-more.netlify.app/gradient/3)
- [/gradient/300](https://alyra-gradients-once-more.netlify.app/gradient/300)

---

## Validation (/10):

**[le site de référence](https://alyra-gradients-bonus.netlify.app/)**

- rendu sur GitHub et déploiement sur Netlify
- `useReducer` avec des actions `FETCH_INIT`, `FETCH_SUCCESS`, `FETCH_FAILURE` est mis en place correctement (/2)
- `GradientsContext` est mis en place correctement (/2)
- la structure des routes est respectée et fonctionne correctement (/2)
- la navigation (Accueil, Précédent, Suivant) depuis routes `/gradient/:id` fonctionne correctement (/1)
- le bouton "Plein écran" fonctionne (/1)
- les gradients viennent depuis l'API (/1)
- il n'y a pas de warning depuis la console dans VSCode, ni dans la console du navigateur (/1)
