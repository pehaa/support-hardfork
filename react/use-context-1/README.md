# React Context API et <code>useContext</code> hook

Dans la programmation, le mot _context_ peut être interprété comme "l'information pertinente". On utilise _context_ dans React si une information est pertinente pour plusieurs components en même temps, à plusieurs niveau de l'arborescence.



> Dans une application React typique, les données sont passées de haut en bas (du parent à l’enfant) via les props, mais cela peut devenir lourd pour certains types de props (ex. les préférences régionales, le thème de l’interface utilisateur) qui s’avèrent nécessaires pour de nombreux composants au sein d’une application. L'API Context offre un moyen de partager des valeurs comme celles-ci entre des composants sans avoir à explicitement passer une prop à chaque niveau de l’arborescence.

> En utilisant l'API Context, nous pouvons éviter de passer les props à travers des éléments intermédiaires.


![](https://wptemplates.pehaa.com/assets/alyra/context.png)

## Exemple 1 (Gradients et FilterContext)

Nous allons découvrir l’API Context de React en procédant à un refactoring. Notre point de départ est [ce repo GitHub](https://github.com/pehaa/alyra-gradients-context).
Nous allons retravailler l’application Alyra Gradients avec une approche alternative, en utilisant API Context.

---

⚠️ Attention ! : Nous avons choisi une application peu complexe qui **ne nécessite pas** du tout l'utilisation de l'API Context. Par contre avec une basse complexité nous pouvons plus facilement comprendra la mise en place et le fontionnement du contexte.

---

### Motivation

Nous utilisons `filter` et `setFilter` dans `GradientsSelect` et `GradientsTagButton`.
Nous utilisont `filter` dans `GradientsList`

Leur common parent est le component `Gradients` et c'est là où nous mettons en place la variable du state :

```javascript
// src/components/Gradients.js
import { useState } from "react"
// ....
const Gradients = () => {
  const [filter, setFilter] = useState("all")
  // ...
}
export default Gradients
```

Ensuite nous passons `filter` et `setFilter` en tant que props dans `GradientsSelect` et `Gradients List`.

Nous devons aussi poursuivre tout le chemin entre `Gradients` vers `GradientTagButton` en passant à chaque fois des props `filter` et `setFilter`.

![](https://wptemplates.pehaa.com/assets/alyra/diagram.png)

---

### Solution alternative avec l'API Context

![](https://wptemplates.pehaa.com/assets/alyra/diagram-usecontext.png)

---

### Mise en place de Context et son "Provider"

```bash
mkdir src/context
touch src/context/FilterContext.js
```

```javascript
// src/context/FilterContext.js
import { useState, createContext } from "react"

// créer et exporter ("named") FilterContext object
export const FilterContext = createContext()

/* le component-provider qui embrassera la partie de notre app où on utilise ce context */
export const FilterContextProvider = ({ children }) => {
  const [filter, setFilter] = useState("all")
  return (
    <FilterContext.Provider value={{ filter, setFilter }}>
      {children}
    </FilterContext.Provider>
  )
}
```

Fichier `FilterContext.js` exporte `FilterContext` (named export) et `FilterContextProvider` (default export).

La props `value` de `Provider` permettra à tous ses components enfants d'avoir accès à la valeur passée. Les components enfant vont aussi se mettre à jour (re-render) quand `value` change.

Ici nous passons dans value un objet avec 2 clés : `filter` et `setFilter`.

### App.js

Dans `App.js`, nous allons importer `FilterContextProvider` et le mettre en place.

```javascript
// src/App.js
import React from "react"
import Gradients from "./components/Gradients"
import GradientsHeader from "./components/GradientsHeader"
import Footer from "./components/Footer"
import { FilterContextProvider } from "./context/FilterContext"

function App() {
  return (
    <>
      <GradientsHeader>
        <h1 className="display-1">Alyra Gradients</h1>
        <p className="tagline">Ultime collection de plus beaux dégradés</p>
      </GradientsHeader>
      <main className="container">
        <h1 className="text-center my-4">Alyra Gradients</h1>
        <FilterContextProvider>
          <Gradients />
        </FilterContextProvider>
      </main>
      <Footer />
    </>
  )
}

export default App
```

### Gradients.js

Nous allons également modifier `Gradients.js`

```javascript
import React from "react"
import GradientsList from "./GradientsList"
import GradientsSelect from "./GradientsSelect"

const Gradients = () => {
  return (
    <>
      <GradientsSelect />
      <GradientsList />
    </>
  )
}

export default Gradients
```

![](https://wptemplates.pehaa.com/assets/alyra/diagram-usecontext.png)

### Consommer context avec `useContext`

`useContext` est un hook React qui permet de consommre le contexte. `useContext` accepte un objet contexte (dans notre cas se sera `FilterContext`), et retourne la valeur actuelle du contexte. 

La valeur de `useContext(FilterContext)` est déterminée par la prop `value` du plus proche <FilterContext.Provider> au-dessus du composant dans l’arbre.

Quand le plus proche <FilterContext.Provider> au-dessus du composant est mis à jour, `useContext` déclenche un re-render avec la value la plus récente passée au `MyContext.Provider`.

#### GradientsList.js

```javascript
// src/components/GradientsList.js
import { useContext } from "react"
import { gradients } from "./../gradients"
import Gradient from "./Gradient"
import { FilterContext } from "./../context/FilterContext"

const GradientsList = props => {
  const { filter } = useContext(FilterContext)
  const list = gradients.filter(el => {
    if (filter === "all") {
      return true
    }
    return el.tags.includes(filter)
  })
  return (
    <ul className="row list-unstyled">
      {list.map(el => {
        const { name, start, end, tags = [] } = el
        return (
          <Gradient
            key={name}
            colorStart={start}
            colorEnd={end}
            name={name}
            tags={tags}
          />
        )
      })}
    </ul>
  )
}

export default GradientsList
```

#### Gradient, GradientTags

Nous allons enlever `filter` et `setFilter` en tant que props.

#### GradientTagButton

```javascript
// src/components/Gradient/GradientTagButton.js
import { useContext } from "react"
import { FilterContext } from "./../../context/FilterContext"

const GradientTagButton = ({ tag }) => {
  const { filter, setFilter } = useContext(FilterContext)
  // comme avant
}

export default GradientTagButton
```

#### GradientsSelect

```javascript
// src/components/GradientsSelect.js
import { useContext } from "react"
import { uniqueTags } from "../gradients"
import { FilterContext } from "./../context/FilterContext"

const GradientsSelect = () => {
  const { filter, setFilter } = useContext(FilterContext)
  // comme avant
}

export default GradientsSelect
```

---

## Custom hook

Au lieu d'importer à chaque fois `useContex` et le Context lui même nous pouvons créer et utiliser notre propre hook :

```js
// src/context/FilterContext.js
import { createContext, useState, useContext } from "react"

// créer FilterContext object
const FilterContext = createContext()

/* le component-provider qui embrassera la partie de notre app où on utilise ce context */

export const useFilter = () => {
  const context = useContext(FilterContext)
  if (context === "undefined") {
    throw new Error(
      `It seems that you are trying to use FilterContext outside of its provider`
    )
  }
  return context
}

export const FilterContextProvider = ({ children }) => {
  const [filter, setFilter] = useState("all")
  return (
    <FilterContext.Provider value={{ filter, setFilter }}>
      {children}
    </FilterContext.Provider>
  )
}
```

## Context recap :

- _contexte_ = information pertinente
- utile lorsque plusieurs components doivent partager la même information (thème, langue, devise)
- permettent d'éviter passer les props plusieurs niveaux plus bas
- Étapes de création :
  - créer le contexte `createContext`
  - créer le component `Provider` avec la props `value` que va tenir l'information pertinente
  - exporter le contexte et son provider
- Pour utiliser le contexte
  - Emballer vos components avec le provider
  - Dans les fichiers components, importer le contexte et passez-le au hook `useContext`

---

## Exemple 2 useReducer & Context

Voici notre [repo de depart.](https://github.com/pehaa/alyra-todos-context)

Nous allons commencer par le refactoring vers `useReducer`.

https://codepen.io/alyra/pen/oNxZBmb

Ensuite nous allons mettre en place `TodosDispatchContext`

```js
// src/context/TodosDispatchContext.js
import { createContext, useContext } from "react"

export const TodosDispatchContext = createContext()

export const useTodosDispatch = () => {
  const context = useContext(TodosDispatchContext)
  if (context === "undefined") {
    throw new Error("useTodosDispatch within TodosDispachContext.Provider")
  }
  return context
}
```

```js
import { useEffect, useState, useReducer } from "react"
import TodosList from "./TodosList"
import SelectTodos from "./SelectTodos"
import AddTodoForm from "./AddTodoForm"
import { todosReducer } from "../reducers/todosReducer"
import { TodosDispatchContext } from "../context/TodosDispatchContext"

const Todos = () => {
  const initialTodos = []
  const [todos, dispatch] = useReducer(
    todosReducer,
    initialTodos,
    initialTodos => {
      return (
        JSON.parse(window.localStorage.getItem("my-new-todos")) || initialTodos
      )
    }
  )

  useEffect(() => {
    window.localStorage.setItem("my-new-todos", JSON.stringify(todos))
  }, [todos])

  return (
    <main>
      <h2 className="text-center">Ma liste de tâches ({todos.length})</h2>
      <TodosDispatchContext.Provider value={dispatch}>
        <TodosList todos={todos} />
        <AddTodoForm />
      </TodosDispatchContext.Provider>
    </main>
  )
}
```

---

## Exercices :

- Mettre en place `DarkModeContext` (`value={darkMode}`). Utiliser la valeur darkMode du contexte dans le component `AddTodoForm` afin de changer les couleurs du formulaire. 
- Familiarisez-vous avec [React Router](https://reactrouter.com/web/guides/quick-start)
    - Installer CRA (1) et mettre en place un simple système de routes - comme dans [1st-example-basic-routing](https://reactrouter.com/web/guides/quick-start/1st-example-basic-routing)
    - Deployer sur Netlify

    Attention - avant de deployer sur Netlify, vous devez configurer les redirects :

    ```bash
    touch public/_redirects
    ```

    ```bash
    # public/_redirects
    /*    /index.html   200
    ```
