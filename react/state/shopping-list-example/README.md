# Shopping List (1)

## Objectif

Nous allons créer une [liste des courses](https://alyra-shopping-list.netlify.app/). L'utilisateur a deux possibilités d'ajouter des produits. Il ajoute des produits à acheter via un simple formulaire. Il peut aussi ajouter un des produits présélectionnés (produits "populaires", dans notre cas pain, lait, pizza, salade ou oranges) en cliquant sur le bouton.

Ensuite, en faisant ces courses, l'utilisateur peut enlever un produit de la liste en cliquant le bouton "✖️ ok".

Nous allons mettre en pratique, ce que nous avons appris jusqu'à ce moment, en particulier :

- React componenents,
- Create React App,
- le hook `React.useState`,
- Bootstrap v5

## Installation

```
npx create-react-app alyra-shopping-list
cd alyra-shopping-list
yarn add bootstrap@next @fontsource/kiwi-maru
```

```js
// ./src/index.js
import React from "react"
import ReactDOM from "react-dom"
import "bootstrap/dist/css/bootstrap.css"
import "@fontsource/kiwi-maru"
import "./index.css"
/* ... */
```

```css
/* ./src/index.css */
body {
  font-family: "Kiwi Maru", sans-serif;
}
```

## Structure du projet

```bash
src
├── App.js
├── components
│   ├── AddProductForm.js
│   ├── AddPopularProduct.js
│   ├── Header.js
│   ├── Product.js
│   ├── ShoppingApp.js
│   └── ShoppingList.js
...
```

```bash
mkdir src/components
touch src/components/AddProductForm.js
touch src/components/AddPopularProduct.js
touch src/components/Header.js
touch src/components/Product.js
touch src/components/ShoppingApp.js
touch src/components/ShoppingList.js
```

![](https://wptemplates.pehaa.com/assets/alyra/shopping-app.png)

## Static markup (bootstrap)

```html
<div class="container">
  <header class="text-center my-5">
    <h1>Ma liste des courses</h1>
    <p lang="en">Let's go shopping! Yay !!</p>
  </header>
  <main class="row">
    <section class="col-lg-8">
      <h2 class="mb-3 h4">Produits à acheter (1):</h2>
      <ul class="list-group mb-3 shadow">
        <li class="list-group-item">
          <div class="d-flex align-items-center justify-content-between">
            cumin
            <button class="btn btn-sm btn-warning">ok</button>
          </div>
        </li>
      </ul>
    </section>
    <section class="col-lg-4">
      <div class="bg-light border p-4">
        <h2 class="mb-3 h4">Ajouter un produit :</h2>
        <form class="mb-5">
          <div class="input-group mb-2">
            <input
              id="product"
              class="form-control"
              aria-label="Ajouter sur la liste"
              required=""
            /><button type="submit" class="btn btn-success btn-lg">
              J'ajoute !
            </button>
          </div>
        </form>
        <section>
          <h3 class="h5">Avez vous besoin de ?</h3>
          <div class="mb-3 d-flex flex-wrap align-items-center">
            <button
              class="btn btn-outline-success me-2 mb-2 d-flex align-items-center"
            >
              pain
              <span class="fs-1" role="img" aria-hidden="true">🥖</span>
            </button>
          </div>
        </section>
      </div>
    </section>
  </main>
</div>
```

https://codepen.io/alyra/pen/dyNWJYK?editors=1010

![](https://wptemplates.pehaa.com/assets/alyra/shopping-list-topo.png)

### App.js

```javascript
// src/App.js
import ShoppingApp from "./components/ShoppingApp"
import Header from "./components/Header"

function App() {
  return (
    <div className="container">
      <Header />
      <ShoppingApp />
    </div>
  )
}

export default App
```

### Header.js

```js
// src/components/Header.js
const Header = () => {
  return (
    <header className="text-center my-5">
      <h1>Ma liste des courses</h1>
      <p lang="en">Let's go shopping! Yay !!</p>
    </header>
  )
}

export default Header
```

### ShoppingApp.js

```js
// src/components/ShoppingApp.js
import ShoppingList from "./ShoppingList"
import AddPopularProduct from "./AddPopularProduct"
import AddProductForm from "./AddProductForm"

const ShoppingApp = () => {
  return (
    <main className="row">
      <section className="col-lg-8">
        <ShoppingList />
      </section>
      <section className="col-lg-4">
        <div className="bg-light border p-4">
          <h2 className="mb-3 h4">Ajouter un produit :</h2>
          <AddProductForm />
          <AddPopularProduct />
        </div>
      </section>
    </main>
  )
}

export default ShoppingApp
```

### ShoppingList

Dans notre application nous allons prendre soin d'avoir la liste des produits uniques. Grâce à cela nous pouvons utiliser `{product}` pour l'attribut `key`.

```js
// src/components/ShoppingList.js
import Product from "./Product"

const ShoppingList = (props) => {
  const shopping = ["cumin", "curry"]
  return (
    <>
      <h2 className="mb-3 h4">Produits à acheter ({shopping.length}):</h2>
      <ul className="list-group mb-3 shadow">
        {shopping.map((product) => {
          return (
            <li className="list-group-item" key={product}>
              <Product product={product} />
            </li>
          )
        })}
      </ul>
    </>
  )
}

export default ShoppingList
```

### Product

```js
// src/components/Product.js
const Product = (props) => {
  const { product } = props

  return (
    <div className="d-flex align-items-center justify-content-between">
      {product}
      <button className="btn btn-sm btn-warning">
        <span role="img" aria-hidden>
          ✖️
        </span>{" "}
        ok
      </button>
    </div>
  )
}

export default Product
```

### AddProductForm

```js
// src/components/AddProductForm.js
const AddProductForm = (props) => {
  return (
    <form className="mb-5" onSubmit={handleFormSubmit}>
      <div className="input-group mb-2">
        <input
          id="product"
          className="form-control"
          aria-label="Ajouter sur la liste"
          required
        />
        <button type="submit" className="btn btn-success btn-lg">
          J'ajoute !
        </button>
      </div>
    </form>
  )
}

export default AddProductForm
```

### AddPopularProduct

```js
// src/components/AddPopularProduct.js
const AddPopularProduct = (props) => {
  const populars = [
    { text: "pain", emoji: "🥖" },
    { text: "lait", emoji: "🥛" },
    { text: "pizza", emoji: "🍕" },
    { text: "salade", emoji: "🥬" },
    { text: "oranges", emoji: "🍊" },
  ]
  return (
    <section>
      <h3 className="h5">Avez vous besoin de ?</h3>
      <div className="mb-3 d-flex flex-wrap align-items-center">
        {populars.map((el) => (
          <button
            key={el.text}
            className="btn btn-outline-success me-2 mb-2 d-flex align-items-center"
          >
            {el.text}{" "}
            <span className="fs-1" role="img" aria-hidden>
              {el.emoji}
            </span>
          </button>
        ))}
      </div>
    </section>
  )
}

export default AddPopularProduct
```

## Variable de state

Le but de cette application est d'afficher la liste des courses. Cette liste est alimentée à chaque fois où un nouveau produit est ajouté via le formulaire. On peut aussi retirer chaque produit de la liste en cliquant sur son bouton "done!"

Notre variable de state (appelons-la `shopping`) sera un array avec des éléments de type `"string"`.

Nous allons la mettre en place comme ceci :

```js
const [shopping, setShopping] = React.useState([])
```

Mais où (dans quel fichier) doit-on placer ce code ?

---

## _Lifting State Up_

React est conçu de cette façon que les informations passent dans un seul sens, uniqument depuis le component parent vers les components enfants. En apelle cela _unidirectional data flow_. Un component parent passe les informations à ses enfants via les props. Quand nous mettons en place une variable de state dans un component, son component parent n'aura pas d'accès à cette variable.

```js
const Article = () => {
  return (
    <article>
      <h2>Votre super article</h2>
      <p>Nous avons déjà ... 🤔😳 likes </p>
      <LoveButton {/* compte le nombre de likes*/} />
    </article>
  )
}
```

Nous allons alors définir la variable de state si haut dans l'arborescence que c'est nécessaire et utiliser props pour la passer plus bas.

> Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor. Let’s see how this works in action.

> [fr] Plusieurs composants ont souvent besoin de refléter les mêmes données dynamiques. Nous conseillons de faire remonter l’état partagé dans leur ancêtre commun le plus proche. Voyons comment ça marche.

```js
const Article = () => {
  const [likes, setLikes] = React.useState(0)
  return (
    <article>
      <h2>Votre super article</h2>
      <p>Nous avons déjà {likes} 🤔😳 likes </p>
      <LoveButton likes={likes} setLikes={setLikes} />
    </article>
  )
}
```

https://codepen.io/alyra/pen/poRPLwm

![](https://wptemplates.pehaa.com/assets/alyra/state-lifiting.png)

---

## State in ShoppingApp

![](https://wptemplates.pehaa.com/assets/alyra/shopping-app-common.png)

```javascript
import { useState } from "react"
import AddPopularProduct from "./AddPopularProduct"
import ShoppingList from "./ShoppingList"
import AddProductForm from "./AddProductForm"

const ShoppingApp = () => {
  const [shopping, setShopping] = useState([])

  const addToShoppingList = (product) => {
    setShopping([...shopping, product])
  }

  const removeFromShoppingList = (product) => {
    setShopping(shopping.filter((el) => el !== product))
  }
  return (
    <main className="row">
      <section className="col-lg-8">
        <ShoppingList
          shopping={shopping}
          removeFromShoppingList={removeFromShoppingList}
        />
      </section>
      <section className="col-lg-4">
        <div className="bg-light border p-4">
          <h2 className="mb-3 h4">Ajouter un produit :</h2>
          <AddProductForm
            shopping={shopping}
            addToShoppingList={addToShoppingList}
          />
          <AddPopularProduct
            shopping={shopping}
            addToShoppingList={addToShoppingList}
          />
        </div>
      </section>
    </main>
  )
}

export default ShoppingApp
```

Ensuite nous allons utiliser `shopping` et `setShopping` dans `ShoppingList` et ensuite `Product`

```js
const ShoppingList = (props) => {
  const { shopping, removeFromShoppingList } = ["cumin", "curry"]
  return (
    <>
      <h2 className="mb-3 h4">Produits à acheter ({shopping.length}):</h2>
      <ul className="list-group mb-3 shadow">
        {shopping.map((product) => {
          return (
            <li className="list-group-item" key={product}>
              <Product
                product={product}
                removeFromShoppingList={removeFromShoppingList}
              />
            </li>
          )
        })}
      </ul>
    </>
  )
}
```

```js
const Product = (props) => {
  const { product, removeFromShopping } = props

  const handleButtonClick = () => {
    removeFromShopping(product)
  }

  return (
    <div className="d-flex align-items-center justify-content-between">
      {product}
      <button className="btn btn-sm btn-warning" onClick={handleButtonClick}>
        <span role="img" aria-hidden>
          ✖️
        </span>{" "}
        ok
      </button>
    </div>
  )
}
```

## Formulaire et `onSubmit`

Nous allons maintenant donner la possibilité d'ajouter des produits notre liste via le formulaire. Pour ceci nous devons mettre en place un _handler_ d'événement `submit` à notre élément `form`, `onSubmit={handleFormSubmit}`. En même temps nous devons définir la fonction `handleFormSubmit`.

```javascript
const AddProductForm = (props) => {
  const { shopping, addToShoppingList } = props
  const handleFormSubmit = (event) => {
    // nous devons empêcher l'action par défaut de notre formulaire
    event.preventDefault()
    // récupérer la valeur depuis le champ input#product
    const newProduct = event.target.elements.product.value
    // s'assurer que la liste contient des produits uniques
    if (shopping.includes(newProduct)) {
      alert(`${newProduct} est déjà sur la liste`)
    } else {
      addToShoppingList(newProduct)
    }
    // vider l'input (remettre le formulaire à zéro)
    event.target.reset()
  }
  return <form onSubmit={handleFormSubmit}>{/* ... */}</form>
}
```

## AddPopularProduct

```js
const AddPopularProduct = (props) => {
  const { shopping, addToShoppingList } = props
  const populars = [...]

  return (
    <section>
      <h3 className="h5">Avez vous besoin de ?</h3>
      <div className="mb-3 d-flex flex-wrap align-items-center">
        {populars.map((el) => (
          <button
            key={el.text}
            className="btn btn-outline-success me-2 mb-2 d-flex align-items-center"
            onClick={() => addToShoppingList(el.text)}
            disabled={shopping.includes(el.text)}
          >
            {/* ... */}
          </button>
        ))}
      </div>
    </section>
  )
}
```

## Bonus - Filtering - Recherche dans les produits de la liste

```js
const ShoppingList = (props) => {
  const { shopping, removeFromShoppingList } = props
  const [filter, setFilter] = useState(""))

  const filteredList = shopping.filter((el) =>
    el.trim().toLowerCase().startsWith(filter.trim().toLowerCase())
  )
  return (
    <>
      <h2 className="mb-3 h4">Produits à acheter ({shopping.length}) :</h2>
      <div className="input-group mb-3">
        <span role="img" aria-label="search" className="input-group-text">
          🔎
        </span>
        <input
          type="search"
          value={filter}
          onChange={(e) => setFilter(e.target.value)}
          placeholder="Rechercher dans votre liste des courses ..."
          aria-label="Chercher"
          className="form-control"
        />
      </div>
      {filter && (
        <p className="d-flex justify-content-between">
          <span>
            Produits qui commencent par <i>{filter}</i>
          </span>
          <button
            className="btn btn-light btn-sm"
            onClick={() => setFilter("")}
          >
            <span role="img" aria-hidden>
              🔄
            </span>{" "}
            Réinitialiser
          </button>
        </p>
      )}
      <ol className="list-group mb-3 shadow">
        {filteredList.map((el) => {
          return (
            <li className="list-group-item" key={el}>
              <Product
                product={el}
                removeFromShoppingList={removeFromShoppingList}
              />
            </li>
          )
        })}
      </ol>
    </>
  )
}

export default ShoppingList
```
