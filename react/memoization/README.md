# Memoization in React

Nous allons parler de

- React hook `useCallback`
- React hook `useMemo`
- Higher Order Component `memo`

## `useCallback`

```js
const memoizedDoSomething = useCallback(() => {
  doSomething(a, b)
}, [a, b])
```

- prend toujours 2 paramètres
- le premier est une fonction à _memoize_
- le deuxième est un array de deps
  - `[]` : function est _memoized_ une seule fois quand le component mounts
  - `[a, b]` : nouvelle _version_ est _memoized_ à chaque fois que la changement de `a` ou de `b` est detecté.

### Pourquoi ?

https://codepen.io/alyra/pen/WNpXOyP

Voici un schema d'une situation type:

```js
const ParentComponent = () => {
  // ici pour la simplicité, state est un nombre
  const [state, setState] = useState(0)
  const someFunction = () => {
    /* qui à l'occasion met à jour state */
    setState(s => s + 1)
  }
  return <ChildComponent someFunction={someFunction} />
}
```

```js
const ChildComponent = ({ someFunction }) => {
  React.useEffect(() => {
    someFunction()
  }, [someFunction])
  return <div>Hello !</div>
}
```

```
ParentComponent mounts (state => 0)
  someFunction is created 🆕
  ChildCompoment mounts
    useEffect runs
      state => 1

---

ParentComponent re-renders (state changed)
  someFunction is created 🆕
  ChildCompoment rerenders
    useEffect runs because someFunction 🆕 changed
      state => 2

---

ParentComponent re-renders (state changed)
  someFunction is created 🆕
  ChildCompoment rerenders
    useEffect runs because someFunction 🆕 changed
      state => 3

---

.... infinite loop 🤯
```

https://codepen.io/alyra/pen/Rwpjgmy

### Solution

```js
const ParentComponent = () => {
  // ici pour la simplicité, state est un nombre
  const [state, setState] = useState(0)
  const someFunction = useCallback(() => {
    /* qui à l'occasion met à jour state */
    setState(s => s + 1)
  }, [])
  return <ChildComponent someFunction={someFunction} />
}
```

```js
const ChildComponent = ({ someFunction }) => {
  React.useEffect(() => {
    someFunction()
  }, [someFunction])
  return <div>Hello !</div>
}
```

```
ParentComponent mounts (state => 0)
  someFunction is created 🆕 and stored ♻ with useCallback
  ChildCompoment mounts
    useEffect runs
      state => 1

---

ParentComponent re-renders (state changed)
  someFunction is restored ♻ from useCallback
  ChildCompoment rerenders
    useEffect does not run because someFunction ♻ didn't change


```

### Star Wars Planets

```js
// Parent Component
import { useReducer } from "react"
import Planets from "./Planets"
import { reducer } from "../reducers"

const PlanetsApp = () => {
  const initialState = {
    planets: [],
    loading: false,
    error: "",
  }
  const [state, dispatch] = useReducer(reducer, initialState)
  const { planets, loading, error } = state

  const initFetch = () => dispatch({ type: "FETCH_INIT" })
  const fetchPlanets = () => {
    fetch(`https://swapi.dev/api/planets/`)
      .then(response => {
        if (!response.ok) {
          throw new Error(
            `Nous n'avons pas pu lire le registre des planètes, status : ${response.status}`
          )
        }
        return response.json()
      })
      .then(data => {
        dispatch({ type: "FETCH_SUCCESS", payload: data.results })
      })
      .catch(error => {
        dispatch({ type: "FETCH_FAILURE", payload: error.message })
      })
  }
  return (
    <>
      <Planets
        planets={planets}
        initFetch={initFetch}
        fetchData={fetchPlanets}
      />
      {loading && <p className="text-center">loading...</p>}
      {!!error && <p className="alert alert-danger">{error}</p>}
    </>
  )
}

export default PlanetsApp
```

```js
// Child Component
import { useEffect } from "react"
import Planet from "./Planet"

const Planets = ({ planets, initFetch, fetchData }) => {
  useEffect(() => {
    initFetch()
    fetchData()
  }, [])
  return (
    <>
      <div className="row">
        {planets.map(planet => {
          return <Planet key={planet.name} planet={planet} />
        })}
      </div>
    </>
  )
}

export default Planets
```

```
PlanetsApp mounts (state => {...})
  fetchInit is created 🆕
  fetchData is created 🆕
  Planets mounts
    useEffect runs
      fetch data calls dispatch
        state changes {...}

---

ParentComponent re-renders (state changed)
  fetchInit is created 🆕
  fetchData is created 🆕
  Planets mounts
    useEffect runs because fetchInit changed and because fetchData changed
      fetch data calls dispatch
        state changes {...}

---

ParentComponent re-renders (state changed)
  fetchInit is created 🆕
  fetchData is created 🆕
  Planets mounts
    useEffect runs because fetchInit changed and because fetchData changed
      fetch data calls dispatch
        state changes {...}
---

.... infinite loop 🤯
```

### Solution

```js
// Parent Component
import { useReducer, useCallback } from "react"
import Planets from "./Planets"
import { reducer } from "../reducers"

const PlanetsApp = () => {
  const initialState = {
    planets: [],
    loading: false,
    error: "",
  }
  const [state, dispatch] = useReducer(reducer, initialState)
  const { planets, loading, error } = state

  const initFetch = useCallback(() => dispatch({ type: "FETCH_INIT" }), [])
  const fetchPlanets = useCallback(() => {
    fetch(`https://swapi.dev/api/planets/`)
      /* comme avant */
  }, [])

  return ( /* comme avant */ )
}

export default PlanetsApp
```

### Changement des pages

```bash
git checkout page
```

Nous avons ajouter un component `<SwitchPage>` et une variable de state `page` afin de pouvoir visaliser les données de toutes les planètes. Mais ceci ne fonctionne pas correctement ?

Comment corriger le code suivant?

```js
import { useReducer, useState, useCallback } from "react"
import Planets from "./Planets"
import { reducer } from "../reducers"
import SwitchPage from "./SwitchPage"

const PlanetsApp = () => {
  const initialState = {
    planets: [],
    loading: false,
    error: "",
  }
  const [page, setPage] = useState(1)
  const [state, dispatch] = useReducer(reducer, initialState)
  const [active, setActive] = useState(false)
  const { planets, loading, error } = state

  const initFetch = useCallback(() => dispatch({ type: "FETCH_INIT" }), [])

  const fetchPlanets = useCallback(() => {
    fetch(`https://swapi.dev/api/planets/?page=${page}`)
      .then(response => {
        if (!response.ok) {
          throw new Error(
            `Nous n'avons pas pu lire le registre des planètes, status : ${response.status}`
          )
        }
        return response.json()
      })
      .then(data => {
        dispatch({ type: "FETCH_SUCCESS", payload: data.results })
      })
      .catch(error => {
        dispatch({ type: "FETCH_FAILURE", payload: error.message })
      })
  }, [])
  return (
    <>
      {!active ? (
        <button className="btn btn-dark" onClick={() => setActive(true)}>
          Afficher des planètes
        </button>
      ) : (
        <>
          <SwitchPage page={page} setPage={setPage} />
          <Planets
            planets={planets}
            initFetch={initFetch}
            fetchData={fetchPlanets}
          />
        </>
      )}
      {loading && <p className="text-center">loading...</p>}
      {!!error && <p className="alert alert-danger">{error}</p>}
    </>
  )
}

export default PlanetsApp
```

## useCallback dans custom hooks
