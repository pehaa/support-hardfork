# React <code>memo</code>

https://codepen.io/alyra/pen/dyveQmE

Enrober un component dans `React.memo` permet de le mémoïser. React vérifiera si les props sont les mêmes que pour le rendu précédent. Si c'est le cas, React sautera le rafraîchissement du composant en réutilisant son dernier rendu en date.  
Dans certains cas cela améliore la performance.

> `React.memo` ne se préoccupe **que** des modifications de props. Si votre composant enrobée par `React.memo` utilise un Hook `useState` ou `useContext` dans son implémentation, des changements de state ou de contexte entraîneront tout de même un nouveau rendu.

---

`React.memo` est un exemple de _Higher Order Component_ (HOC). HOC est une fonction qui prend un component (alors une fonctions) comme son premier paramètre et retourne un component.

---

Dans le Pen ci-dessus, nous utilisons le hook `useRef` afin de mettre en place une variable qui persiste entre les rendus. Dans notre cas c'est `count` qui nous permet de calculer le nombre des re-rendus de chaque component.

- **Premier render**

  - je mets en place une variable qui persiste  
    `const myPersistentVariable = useRef(initialValue)`
  - React met en place  
    `myPersistentVariable = {current: initialValue}`

- **Chaque render**
  - je peux lire la valeur :  
    `myPersistentVariable.current`
  - je peux modifier la valeur :  
     `myPersistentVariable.current = newValue`

---

## Crypto Currencies - Point de départ

Nous allons démarrer avec [ce repo](https://github.com/pehaa/crypto-memo), vous allez avoir besoin d'une clé API de [nomics cryptocurrency bitcoin API](https://p.nomics.com/cryptocurrency-bitcoin-api).

```js
// src/components/List.js
import { useState } from "react"
import { useCurrencies } from "../hooks/useCurrencies"
import Currency from "./Currency"

const List = () => {
  // useCurrencies est un custom hook avec useEffect et useReducer
  const { error, loading, currencies } = useCurrencies()
  // active est null ou égale à currency id
  // currency active affiche tous les détails
  const [active, setActive] = useState(null)
  // ce n'est pas une bonne idée d'afficher 16k+ résultats
  const displayedCurrencies = currencies.slice(0, 500)

  return (
    <div>
      {loading ? (
        <p>loading... it can take a while...</p>
      ) : (
        <>
          <p>
            {displayedCurrencies.length} first results / {currencies.length}
          </p>
          {displayedCurrencies.map(el => {
            return (
              <Currency
                key={el.id}
                currency={el}
                isActive={el.id === active}
                setActive={setActive}
              />
            )
          })}
        </>
      )}
      {error && <p>{error}</p>}
    </div>
  )
}

export default List
```

```js
// src/components/Currency.js
const Currency = ({ currency, isActive, setActive }) => {
  const { id, name, symbol, logo_url, ...rest } = currency
  return (
    <div>
      <div>
        {logo_url && <img src={logo_url} alt={`${name} logo`} width="32" />}
        <h2>
          <span>{name}</span> (<small>{symbol}</small>)
        </h2>
        {isActive ? (
          <button onClick={() => setActive(null)}>Hide details</button>
        ) : (
          <button onClick={() => setActive(id)}>Show details</button>
        )}
      </div>
      {isActive && <pre>{JSON.stringify(rest, null, 2)}</pre>}
    </div>
  )
}

export default Currency
```

```js
// src/hooks/useCurrencies.js
import { useEffect, useReducer } from "react"
import { reducer } from "../reducers"
export const useCurrencies = () => {
  const [{ loading, error, currencies }, dispatch] = useReducer(reducer, {
    loading: false,
    error: "",
    currencies: [],
  })
  useEffect(() => {
    const API_KEY = process.env.REACT_APP_API_KEY
    dispatch({ type: "FETCH_INIT" })
    fetch(
      `https://api.nomics.com/v1/currencies/ticker?key=${API_KEY}&interval=1d,30d&convert=EUR`
    )
      .then(response => {
        if (!response.ok) {
          throw new Error("Désolée, nous avons rencontré un problème...")
        }
        return response.json()
      })
      .then(data => {
        if (!isCancelled) {
          dispatch({ type: "FETCH_SUCCESS", payload: data })
        }
      })
      .catch(error => {
        console.log(error.message)
        if (!isCancelled) {
          dispatch({ type: "FETCH_FAILURE", payload: error.message })
        }
      })
  }, [])
  return { loading, error, currencies }
}
```

```js
// src/reducers/index.js
export const reducer = (state, action) => {
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
        error: "",
        currencies: action.payload,
      }
    case "FETCH_FAILURE":
      return {
        ...state,
        loading: false,
        error: action.payload,
      }
    default:
      throw new Error("Unsupported action type")
  }
}
```

## Le problème

Nous avons 500 currencies sur la page. Quand une devient _"active"_, le state `active` change dans le component `List`. Toute l'arborescence à partir de `List` est alors _régénérée_.  
Avec un grand nombre d'éléments, ceci peut avoir l'impact sur la performance.

- Pour mieux observer ce problème nous allons "empirer" l'expérience dans l'onglet **Performance** de Dev Tools. Nous allons changer **CPU: No throttling** pour **CPU: 6x slowndown**.
- Nous allons ouvrir l'onglet **Profiler** de React Dev Tools.

<a href="https://wptemplates.pehaa.com/assets/alyra/profile-crypto.mp4" target="_blank" rel="noreferrer noopener">Voici en exemple</a>

---

A chaque fois que `active` change, 500 components `Currency` sont rendus, dont 498 ou 499 sans changement des props, avec le même rendu.

Dans ce cas là, nous pouvons **envisager** l'utilisation de `memo`. Nous allons alors vérifier si `memo` effectivement améliore la performance.

## La solution - `memo`

```js
// src/components/Currency.js
import { memo } from "react"
const Currency = ({ currency, isActive, setActive }) => {
  const { id, name, symbol, logo_url, ...rest } = currency
  return (
    <div>
      <div>
        {logo_url && <img src={logo_url} alt={`${name} logo`} width="32" />}
        <h2>
          <span>{name}</span> (<small>{symbol}</small>)
        </h2>
        {isActive ? (
          <button onClick={() => setActive(null)}>Hide details</button>
        ) : (
          <button onClick={() => setActive(id)}>Show details</button>
        )}
      </div>
      {isActive && <pre>{JSON.stringify(rest, null, 2)}</pre>}
    </div>
  )
}
// souvent on met en place memo ici 👇
export default memo(Currency)
```

<a href="https://wptemplates.pehaa.com/assets/alyra/profile-crypto-memo.mp4" target="_blank" rel="noreferrer noopener">Voici comment la performance a changé</a>

---

## `memo` et `useCallback`

Imaginons maintenant un changement dans le component `List`.

Au lieu de passer `setActive` via props, nous optons pour `hideDetails` et `showDetails` - cela nous semble plus lisible.

Nous appliquons cette modification dans `List` et ensuite dans `Currency`.

```js
// src/components/List.js
import { useState } from "react"
import { useCurrencies } from "../hooks/useCurrencies"
import Currency from "./Currency"

const List = () => {
  const { error, loading, currencies } = useCurrencies()
  const [active, setActive] = useState(null)
  const displayedCurrencies = currencies.slice(0, 500)

  const hideDetails = () => setActive(null)
  const showDetails = id => setActive(id)

  return (
    <div>
      {loading ? (
        <p>loading... it can take a while...</p>
      ) : (
        <>
          <p>
            {displayedCurrencies.length} first results / {currencies.length}
          </p>
          {displayedCurrencies.map(el => {
            return (
              <Currency
                key={el.id}
                currency={el}
                isActive={el.id === active}
                showDetails={showDetails}
                hideDetails={hideDetails}
              />
            )
          })}
        </>
      )}
      {error && <p>{error}</p>}
    </div>
  )
}

export default List
```

```js
// src/components/Currency.js
import { memo } from "react"
const Currency = ({ currency, isActive, showDetails, hideDetails }) => {
  const { id, name, symbol, logo_url, ...rest } = currency
  return (
    <div>
      <div>
        {logo_url && <img src={logo_url} alt={`${name} logo`} width="32" />}
        <h2>
          <span>{name}</span> (<small>{symbol}</small>)
        </h2>
        {isActive ? (
          <button onClick={hideDetails}>Hide details</button>
        ) : (
          <button onClick={() => showDetails(id)}>Show details</button>
        )}
      </div>
      {isActive && <pre>{JSON.stringify(rest, null, 2)}</pre>}
    </div>
  )
}

export default memo(Currency)
```

Nous pouvons vérifier dans Profiler que 500 components `Currency` sont de retours rendu à chaque changement de `active`. Pourquoi ? Quand React compare des props, les 👉**fonctions**👈 `hideDetails` et `showDetails` ne sont jamais les mêmes que pendant le rendu précédent.

Nous pouvons y remédier avec `useCallback`.

```js
// src/components/List.js
import { useState, useCallback } from "react"
import { useCurrencies } from "../hooks/useCurrencies"
import Currency from "./Currency"

const List = () => {
  const { error, loading, currencies } = useCurrencies()
  const [active, setActive] = useState(null)
  const displayedCurrencies = currencies.slice(0, 500)

  const hideDetails = useCallback(() => setActive(null), [])
  const showDetails = useCallback(id => setActive(id), [])

  return /* comme avant */
}

export default List
```
