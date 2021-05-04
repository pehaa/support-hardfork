# <code>useReducer</code> hook

`useReducer` est une alternative à `useState` (plus flexible, plus configurable). `useReducer` Accepte un _reducer_ de type `(state, action) => newState`, et renvoie _current state_ accompagné d’une méthode dispatch.

> `useReducer` est souvent préférable à useState quand vous avez une logique d’état local complexe qui comprend plusieurs sous-valeurs, ou quand l’état suivant dépend de l’état précédent.

Typiquement, nous utilisons `useReducer` pour réunir ensemble la gestion de quelques variables de state qui sont reliées entre elles.

```js
const [stateVar1, setStateVar1] = useState(initialVar1)
const [stateVar2, setStateVar2] = useState(initialVar2)
const [stateVar3, setStateVar3] = useState(initialVar3)
```

```js
const initialState = {
  var1: initialVar1,
  var2: initialVar2,
  var3: initialVar3,
}
const [state, dispatch] = useReducer(reducer, initialState)
```

## Anatomie

![](https://wptemplates.pehaa.com/assets/alyra/usereducer-anatomie.png)

```javascript
const reducer = (state, action) => {
  /* ... */
  // retourne une nouvelle valeur de state
}

const [state, dispatch] = React.useReducer(reducer, initialState, init)

// Ensuite quand nous appellons
dispatch(action)
//React execute
reducer(state, action)
```

- Comme dans l'API de `useState`, `useReducer` retourne un array, le premier élément est notre state, le deuxième est une fonction (`dispatch`).

- Nous appelons la fonction `dispatch` à chaque fois où nous allons mettre à jour `state`. Pourtant la fonction `dispatch` ne modifie pas `state` directement. Elle délègue ceci à la fonction `reducer`.

- La fonction `reducer` retourne une nouvelle valeur de state et si la modification de state est détectée le re-render est déclenché.

- La dénomination `dispatch`, `action` et `reducer` est tout à fait facultative et suit une certaine convention provenant de `Redux`.

## Exemple Counter

```javascript
// avec useState
const Counter = () => {
  const [count, setCount] = React.useState(0)
  return (
    <>
      <button type="button" onClick={() => setCount(count + 1)}>
        -1
      </button>
      <button type="button" onClick={() => setCount(count + 1)}>
        +1
      </button>
      <button type="button" onClick={() => setCount(0)}>
        Reset
      </button>
      {count}
    </>
  )
}
```

https://codepen.io/alyra/pen/LYWPEBO

```javascript
// avec useReducer
const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return state + 1
    case "DECREMENT":
      return state - 1
    case "RESET":
      return 0
    default:
      throw new Error(`Unsupported action type: ${action.type}`)
  }
}
const Counter = () => {
  const [state, dispatch] = React.useReducer(reducer, 0)
  return (
    <>
      <button type="button" onClick={() => dispatch({ type: "DECREMENT" })}>
        -1
      </button>
      <button type="button" onClick={() => dispatch({ type: "INCREMENT" })}>
        +1
      </button>
      <button type="button" onClick={() => dispatch({ type: "RESET" })}>
        Rest
      </button>
      {state}
    </>
  )
}
```

```javascript
/* avant :
setCount(count + 1)
*/
dispatch({ type: "INCREMENT" })
```

```javascript
/* avant :
setCount(count - 1)
*/
dispatch({ type: "DECREMENT" })
```

```javascript
/* avant :
setCount(0)
*/
dispatch({ type: "RESET" })
```

https://codepen.io/alyra/pen/rNyBazx

---

## Exemple - Sandwicherie React

https://codepen.io/alyra/pen/VwPoBev

https://codepen.io/alyra/pen/eYgqLRJ

```javascript
/* avant :
setTaken([...taken, ingredient]);
setToTake(toTake.filter((el) => el !== ingredient));
setPrice(price + 0.5);
*/
dispatch({ type: "ADD", payload: ingredient })
```

```javascript
/* avant :
setTaken(taken.filter((el) => el !== ingredient));
setToTake([...toTake, ingredient]);
setPrice(price - 0.5);
*/
dispatch({ type: "REMOVE", payload: ingredient })
```

## Exemple - Star Wars Planets (<code>useReducer</code> & <code>fetch</code>)

https://codepen.io/alyra/pen/mdWbyYa

```javascript
/* avant
const [planets, setPlanets] = useState([]);
const [loading, setLoading] = useState(false);
const [error, setError] = useState("");
const [page, setPage] = useState(1);
const [hasNext, setHasNext] = useState(null);
*/
const initialState = {
  planets: [],
  loading: false,
  error: "",
  page: 1,
  hasNext: null,
}
const [state, dispatch] = useReducer(reducer, initialState)
```

Notre fonction `reducer` supportera 4 actions

- `"FETCH_INIT"`
- `"FETCH_SUCCESS"`
- `"FETCH_FAILURE"`
- `"NEXT PAGE"`

```javascript
const reducer = (state, action) => {
  switch (action.type) {
    case "FETCH_INIT":
      return {
        ...state,
        loading: true,
      }
    case "FETCH_SUCCESS":
      return {
        ...state,
        loading: false,
        hasNext: !!action.payload.next,
        planets: [...state.planets, ...action.payload.results],
      }
    case "FETCH_FAILURE":
      return {
        ...state,
        error: action.payload,
      }
    case "NEXT PAGE":
      return {
        ...state,
        page: state.page + 1,
      }
    default:
      throw new Error(`Unsupported action type ${action.type}`)
  }
}
```

```javascript
/* avant :
setLoading(true);
*/
dispatch({ type: "FETCH_INIT" })
```

```javascript
/* avant :
setLoading(false);
setPlanets([...planets, ...data.results]);
setHasNext(!!data.next);
*/
dispatch({ type: "FETCH_SUCCESS", payload: data })
```

```javascript
/* avant :
setError(error.message)
setLoading(false)
*/
dispatch({ type: "ERROR", payload: error.message })
```

```javascript
/* avant :
setPage(page + 1)
*/
dispatch({ type: "NEXT_PAGE" })
```

Vous trouverez le code complet ici :

https://codepen.io/alyra/pen/ZEezYgB

---

## "Lazy initial state"

Vous vous rappelez de "lazy initial state" dans l'API de `useState` ?

```javascript
// pas très bien 👎
const [shopping, setShopping] = useState(expensiveOperationFunction())
```

React a besoin du résultat de `expensiveOperationFunction` une fois seulement, lorsque le component est monté, pour initier `shopping`. Cependant avec le code présenté ci-dessus, la fonction `expensiveOperationFunction` sera exécutée à chaque re-render.

Pour remédier à ce problème et empêcher la re-évaluation d'une fonction coûteuse, nous passons **cette fonction** en tant qu' "initial state".

```javascript
// bien 👍 ici nous passons la fonction, mais nous ne l'appelons pas
const [shopping, setShopping] = useState(expensiveOperationFunction)
```

**L'API de `useReducer`** nous permet également d'initier state uniquement au premier "render". Pour cela nous devons utiliser le 3 argument de `useReducer`, par exemple :

```javascript
const init = (initialState) => {
  // initialState = le 2e param de useReducer
  if (localStorage.getItem("shopping")) {
    return JSON.parse(localStorage.getItem("shopping"))
  }
  return initialState
}
const [shopping, dispatch] = React.useReducer(reducer, [], init)
```

---

## Implementer `useState` avec `useReducer`

Nous pouvons observer que le code suivant :

```javascript
const [count, setCount] = useState(0)
```

est équivalent à :

```js
const reducer = (count, newCount) => newCount
const [count, setCount] = React.useReducer(reducer, 0)
```

```js
const reducer = (state, action) => action
const useState = (initialState) => {
  return React.useReducer(reducer, initialState)
}
```

Voici l'implementanion complete prennant en compte la possibilité de passer des fonctions en tant qu'initial state ou dans la fonction "update" :

```javascript
const reducer = (state, action) => {
  if (typeof action === "function") {
    return action(state)
  }
  return action
}

const init = (initialState) => {
  if (typeof initiaState === "function") {
    return initialState()
  }
  return initialState
}

const useState = (initialState) => {
  return React.useReducer(reducer, initialState, init)
}
```

---
