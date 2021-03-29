# Shopping List - live coding

## Introduction et prérequis

Dans ce cours, nous allons mettre en pratique, ce que nous avons appris, en particulier `useState`. Nous allons créer une _shopping list_ - une liste des courses interactive. Nous allons démarrer avec [le code suivant (lien vers le pen de départ).](https://codepen.io/alyra/pen/PoNQWGJ)

```html
<div id="root"></div>
<script crossorigin src="https://unpkg.com/react/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom/umd/react-dom.development.js"></script>
```

```javascript
const ShoppingApp = () => {
  return (
    <section>
      <h2>My shopping List</h2>
      <ol></ol>
      <AddProductForm />
    </section>
  )
}

const AddProductForm = (props) => {
  return (
    <form>
      <div className="input-group mb-2">
        <label className="input-group-text" htmlFor="product">
          Ajouter sur la liste
        </label>
        <input className="form-control" id="product" required="" />
      </div>
      <button type="submit" className="btn btn-primary">
        J'ajoute !
      </button>
    </form>
  )
}

const App = () => {
  return (
    <div className="container my-3">
      <ShoppingApp />
    </div>
  )
}

ReactDOM.render(<App />, document.getElementById("root"))
```

Et, comme d'habitude nous utilisons le fichier style de boostrap5.

https://codepen.io/alyra/pen/PoNQWGJ

## Variable de state


Le but de cette application est d'afficher la liste des courses. Cette liste est alimentée à chaque fois où un nouveau produit est ajouté via le formulaire. On peut aussi retirer chaque produit de la liste en cliquant sur son bouton "done!"

Notre variable de state (appelons-la `shopping`) sera un array avec des éléments de type `"string"`. Mettons-la en place :


```javascript
const ShoppingApp = () => {
  const [shopping, setShopping] = useState(["cumin", "curry", "poivre"])
  return (
    <section>
      <h2>My shopping List</h2>
      <ol></ol>
      <AddProductForm />
    </section>
  )
}
```

La valeur initiale peut être un array vide `[]`, ici je l'ai préremplie de quelques produits afin qu'on puisse plus rapidement travailler avec le rendu.

## Rendu des produits de la liste `shopping`

Les produits dans `shopping` seront affichés au sein de la liste numérotée `ol`.
Le markup pour chaque produit est prévu comme ceci :

```html
<li class="mb-2">
  <div class="d-flex align-items-center justify-content-between">
    lait
    <button type="button" class="btn btn-sm btn-warning">done!</button>
  </div>
</li>
```

Nous allons parcourir notre *array* `shopping` avec la méthode `.map` :

```javascript
const ShoppingApp = () => {
  const [shopping, setShopping] = useState(["cumin", "curry", "poivre"])
  return (
    <section>
      <h2>My shopping List</h2>
      <ol>
        {shopping.map((product) => {
          /* 
            Pour chaque élément de la liste nous allons utiliser le markup prévu pour <li>
            Pour le mettre compatible avec jsx nous devons :
            - changer class pour className
            - ajouter l'attribut spécial key
            - remplacer lait  par {product}
          */
          <li key={product} className="mb-2">
            <div className="d-flex align-items-center justify-content-between">
              {product}
              <button type="button" className="btn btn-sm btn-warning">
                done!
              </button>
            </div>
          </li>
        })}
      </ol>
      <AddProductForm />
    </section>
  )
}
```

Dans notre application nous allons prendre soin d'avoir la liste des produits uniques. Grâce à cela nous pouvons utiliser `{product}` pour l'attribut `key`.

## Formulaire et `onSubmit`

Nous allons maintenant donner la possibilité d'ajouter des produits notre liste via le formulaire. Pour ceci nous devons mettre en place un *handler* d'événement `submit` à notre élément `form`, `onSubmit={handleFormSubmit}`. En même temps nous devons définir la fonction `handleFormSubmit`.

```javascript
const AddProductForm = (props) => {
  const { shopping, setShopping } = props
  const handleFormSubmit = (event) => {
    // nous devons empêcher action par défaut de notre formulaire, js prend la relève !
    event.preventDefault()
    // récupérer la valeur depuis le champ input#product
    const newProduct = event.target.elements.product.value
    // s'assurer que la liste contient des produits uniques
    if (shopping.includes(newProduct)) {
      alert(`${newProduct} est déjà sur la liste`)
    } else {
      // on n'a pas le droit d'utiliser push sur shopping !!!! puisque push modifie shopping, nous devons retourner une nouvelle array
      setShopping([...shopping, newProduct])
    }
    // vider l'input (remettre le formulaire à zéro)
    event.target.reset()
  }
  return (
    <form onSubmit={handleFormSubmit}>
      <div className="input-group mb-2">
        <label className="input-group-text" htmlFor="product">
          Ajouter sur la liste
        </label>
        <input className="form-control" id="product" required />
      </div>
      <button type="submit" className="btn btn-primary">
        J'ajoute !
      </button>
    </form>
  )
}
```

## Buttons "done!"

Nous allons ajouter `onClick` aux boutons 'done!'. Avec _click_ le produit en question devrait être retiré de l'array `shopping`. Nous allons passer `product` en tant que paramètre dans `handleDoneClick`.

```javascript
<button
  onClick={() => handleDoneClick(product)}
  type="button"
  className="btn btn-sm btn-warning"
>
  done!
</button>
```

```javascript
const ShoppingApp = () => {
  const [shopping, setShopping] = useState(["cumin", "curry", "poivre"])
  return (
    <section>
      <h2>My shopping List</h2>
      <ol>
        {shopping.map((product) => {
          /* 
            jsx pour chaque élément de la liste
            nous allons utilisé le markup de <li>
            - changer class pour className
            - ajouter key
            - remplacer lait  par {product}
          */
          ;<li key={product} className="mb-2">
            <div className="d-flex align-items-center justify-content-between">
              {product}
              <button type="button" className="btn btn-sm btn-warning">
                done!
              </button>
            </div>
          </li>
        })}
      </ol>
      <AddProductForm />
    </section>
  )
}
```

La dernière chose est de définir `handleDoneClick`. Nous pouvons utiliser la méthode `filter`,`shopping.filter((el) => el !== product)` retourne la liste où nous gardons tous les éléments différents que `product`.

```javascript
const handleDoneClick = (product) => {
  setShopping(shopping.filter((el) => el !== product))
}
```

Vous trouverez le code final ici :

https://codepen.io/alyra/pen/jOqYggy

---

## Exercices :

- [State - wallet](https://codepen.io/alyra/pen/LYNeYeL) | [solution](https://codepen.io/alyra/pen/e338ac3c0b89e075037141ae852c6023)
- [State settings](https://codepen.io/alyra/pen/bGpaRJJ) | [solution](https://codepen.io/alyra/pen/2379640ca179a5c55839e338c28b679f)
- [State alien](https://codepen.io/alyra/pen/zYqpdGw) | [solution](https://codepen.io/alyra/pen/b4a10fb49de21fbaa6e0f7daa229ee4c)
- [State shopping list](https://codepen.io/alyra/pen/rNepXRY) | [solution](https://codepen.io/alyra/pen/a4bb96fcc8c2c5dcba3eb2b1720db479)
