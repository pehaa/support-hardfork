# React <code>useMemo</code>

Nous allons démarrer avec [ce repo](https://github.com/pehaa/crypto-memo), la branche `start-useMemo`.

```bash
git checkout start-useMemo
```

## Point de départ

Nous allons installer une librairie externe [match-sorter](https://github.com/kentcdodds/match-sorter)

```bash
yarn add match-sorter
```

```js
// src/components/List.js
import { matchSorter } from "match-sorter"
import { useState } from "react"
import { useCurrencies } from "../hooks/useCurrencies"
import Currency from "./Currency"

const List = () => {
  const { error, loading, currencies } = useCurrencies()

  const [filter, setFilter] = useState("")
  const filteredCurrencies = filter
    ? matchSorter(currencies, filter, { keys: ["name", "symbol"] })
    : currencies
  const displayedCurrencies = filteredCurrencies.slice(0, 500)

  const [active, setActive] = useState(null)
  const hideDetails = useCallback(() => setActive(null), [])
  const showDetails = useCallback(id => setActive(id), [])

  return (
    <div>
      {loading ? (
        <p>loading... it can take a while...</p>
      ) : (
        <>
          <div>
            <label htmlFor="filter">Filter currencies</label>
            <input
              id="filter"
              value={filter}
              onChange={e => setFilter(e.target.value)}
            />
          </div>
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

Nous allons observer la performance de notre application dans Profiler. En particulier, nous allons clicker **Show/Hide details** et observer la différence :

- avec le champs de `input` vide (`filter = ""`)
- avec le champs de `input` rempli (`filter = "bit"`).

## Le problème

Le problème vient de la fonction `matchSorter`, qui prend un temps considerable à executer.  
A chaque rendu de `List` nous recalculons `filteredCurrencies` et enfin `displayedCurrencies`.

Nous n'avons pas besoin de recalculer `displayedCurrencies` à chaque fois que `active` change.

Ce serait bien si on pouvait mémoïser `displayedCurrencies` 🤔.

## La solution - `useMemo`

React hook `useMemo` permet de mémoïser une valeur (de la même façon que `useCallback` permet de mémoïser une fonction).

---

```js
const myMemoizedValue = useMemo(() => {
  /* .... */ return valueToMemoize
}, [deps])
```

---

```js
// src/components/List.js
import { matchSorter } from "match-sorter"
import { useState } from "react"
import { useCurrencies } from "../hooks/useCurrencies"
import Currency from "./Currency"

const List = () => {
  const { error, loading, currencies } = useCurrencies()

  const [filter, setFilter] = useState("")
  const displayedCurrencies = useMemo(() => {
    const filteredCurrencies = filter
      ? matchSorter(currencies, filter, { keys: ["name", "symbol"] })
      : currencies
    return filteredCurrencies.slice(0, 500)
  }, [currencies, filter])

  const [active, setActive] = useState(null)
  const hideDetails = useCallback(() => setActive(null), [])
  const showDetails = useCallback(id => setActive(id), [])

  return /* comme avant */
}

export default List
```

---

`useCallback` est en fait une implementation de `useMemo` :

```js
const myFunction = useCallback(fn, [deps])
```

est equivalent à

```js
const myFunction = useMemo(() => fn, [deps])
```

---

## `useMemo` avec `memo` et Context API

Nous allons introduire `darkMode` dans notre application.

```bash
git checkout start-with-context
```

```js
// src/App.js

import { useState } from "react"
import List from "./components/List"

function App() {
  const [darkMode, setDarkMode] = useState(false)
  return (
    <div className={darkMode ? "bg-dark text-light" : ""}>
      <button onClick={() => setDarkMode(!darkMode)}>change</button>
      <header>
        <h1>
          Crypto Currencies with{" "}
          <a className="text-reset" href="https://nomics.com/docs/">
            nomics API
          </a>
        </h1>
      </header>
      <List />
    </div>
  )
}

export default App
```

Suite à ça notre component `List` est rendu quand `darkMode` change. Ceci n'a pas un impact très fort sur la performance, mais nous pouvons quand même opter à emballer `List` dans `memo`.

```js
// src/components/List.js

import { useState, useCallback, useMemo, memo } from "react"
// comme avant
export default memo(List)
```

Regardons maintenant ce qui va se passer si nous décidons d'utiliser les currencies avec l'API Context.

```js
// src/context/CurrenciesContext.js
import { createContext, useContext } from "react"
import { useCurrencies } from "../hooks/useCurrencies"

export const CurrenciesContext = createContext()

export const CurrenciesContextProvider = ({ children }) => {
  const { error, loading, currencies } = useCurrencies()
  return (
    <CurrenciesContext.Provider value={{ error, loading, currencies }}>
      {children}
    </CurrenciesContext.Provider>
  )
}

export const useCurrenciesContext = () => {
  const context = useContext(CurrenciesContext)
  if (context === "undefined") {
    throw new Error(
      "useActive should not be used outside the CurrenciesContextProvider"
    )
  }

  return context
}
```

Nous allons placer `CurrenciesContextProvider` dans notre component `App`.

```js
import { useState } from "react"
import List from "./components/List"
import { CurrenciesContextProvider } from "./context/CurrenciesContext"

function App() {
  const [darkMode, setDarkMode] = useState(false)
  return (
    <div className={darkMode ? "bg-dark text-light" : ""}>
      <button onClick={() => setDarkMode(!darkMode)}>change</button>
      <CurrenciesContextProvider>
        <header>
          <h1>
            Crypto Currencies with{" "}
            <a className="text-reset" href="https://nomics.com/docs/">
              nomics API
            </a>
          </h1>
        </header>
        <List />
      </CurrenciesContextProvider>
    </div>
  )
}

export default App
```

Vous pouvez observer que `List` (`memo(List)`) _”rerenders”_ quand nous changeons le `darkMode`. Pourquoi ?

`List` _”rerenders”_ quand son state change. Si un component _consomme_ un contexte, il _”rerenders”_ à chaque fois que `value` passée au Provider ce ce contexte change. Et dans notre cas ?

Quand le `darkMode` change dans App, `CurrenciesContextProvider` repasse `{error, loading, currencies}` dans sa props value. Cette valeur est à chaque fois différente, à chaque fois c'est un nouvel objet. Mais nous pouvons le mémoïser avec `useMemo`.

```js
export const CurrenciesContextProvider = ({ children }) => {
  const { error, loading, currencies } = useCurrencies()
  const value = useMemo(() => ({ error, loading, currencies }), [
    error,
    loading,
    currencies,
  ])
  return (
    <CurrenciesContext.Provider value={value}>
      {children}
    </CurrenciesContext.Provider>
  )
}
```
