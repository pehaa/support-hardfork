# Memoization dans React (1)

Nous allons parler de :

- React hook `useCallback`
- React hook `useMemo`
- Higher Order Component `memo`

## `useCallback`

---

**Situation type 1 :**  
Le component reçoit une fonction via props et elle est utilisée dans `useEffect`.

---

**Situation type 2 :**  
Je crée un hook qui retourne une fonction qui peut être utilisée dans `useEffect`.

---

**Situation type 3 :**  
Le component emballé par `React.memo` reçoit une fonction via props.

---

```js
const memoizedDoSomething = useCallback(() => {
  doSomething(a, b)
}, [a, b])
```

- prend toujours 2 paramètres
- le premier est une fonction à _memoize_
- le deuxième est un array de deps
  - `[]` : function est _memoized_ une seule fois quand le component mounts
  - `[a, b]` : nouvelle _version_ est _memoized_ à chaque fois que la changement de `a` ou de `b` est détecté.

### Le problème (Exemple Star Wars Planet)

Regardons ensemble le code dans [ce repo.](https://github.com/pehaa/alyra-planets-usecallback)

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
  }, []) // 👈 ⚠️ warning, nous devons ajouter des deps
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

alors :

```js
// Child Component
import { useEffect } from "react"
import Planet from "./Planet"

const Planets = ({ planets, initFetch, fetchData }) => {
  useEffect(() => {
    initFetch()
    fetchData()
  }, [initFetch, fetchData]) // no more warnings !!!
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

No more warnings 🎉 mais ....

![](https://wptemplates.pehaa.com/assets/alyra/warning.png)

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

![](https://wptemplates.pehaa.com/assets/alyra/warning.png)

https://codepen.io/alyra/pen/Rwpjgmy

### Solution

![](https://wptemplates.pehaa.com/assets/alyra/usecallback.png)

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

Nous avons ajouté un component `<SwitchPage />` et une variable de state `page` afin de pouvoir visualiser les données de toutes les planètes. Mais ceci ne fonctionne pas correctement.

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

### Solution

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
  }, [page]) // 👈
  return /* comme avant */
}

export default PlanetsApp
```

## `useCallback` dans custom hooks

Revenons sur notre exemple [Todo with API.](https://github.com/pehaa/alyra-todos-jwt/tree/final)

Nous allons mettre en place un hook qui permet de controller la fonction `dispatch`, elle devrait être exécutée uniquement si component est dans la page.

```js
// src/hooks/useSafeDispatch.js
import { useCallback } from "react"
import { useIsMounted } from "./useIsMounted"

export const useSafeDispatch = dispatch => {
  const isMounted = useIsMounted()
  return useCallback(
    action => (isMounted.current ? dispatch(action) : undefined),
    [dispatch, isMounted]
  )
}
```
