# Todos avec une base de donnée et l'API

## Mise en place du backend

Clonez [ce repo](https://github.com/pehaa/fake-api)

```bash
cd fake-api
yarn
yarn start
```

Nous allons communiquer avec notre base de donnée (`database.json`) via une API Rest. Nous allons tester notre API avec [Hoppscotch](https://hoppscotch.io/)

<a href="https://wptemplates.pehaa.com/assets/alyra/hoppscotch.mp4" target="_blank" rel="noreferrer noopener">Voici en exemple</a>

### `GET` - avec `fetch`

```js
fetch("http://localhost:4000/todos", {
  method: "GET", // default
})
```

### `POST` - avec `fetch`

```js
fetch("http://localhost:4000/todos", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    text: "Add new todo via API",
    isCompleted: false,
  }),
})
```

### `PUT` - avec `fetch`

```js
fetch("http://localhost:4000/todos/5", {
  method: "PUT",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    text: "Add new todo via API",
    isCompleted: true,
  }),
})
```

### `DELETE` - avec `fetch`

```js
fetch("http://localhost:4000/todos/5", {
  method: "DELETE",
  headers: {
    "Content-Type": "application/json",
  },
})
```

## Application Todos

Vous pouvez démarrer avec [ce repo](https://github.com/pehaa/alyra-todos-with-api)

Nous allons commencer par la modification de notre `todosReducer`. Vous vous rappelez du useReducer pour les Star Wars Planets 🌌 ?

```js
// src/components/Todos.js
// après
const initialState = {
  todos: [],
  loading: false,
  error: "",
}
const [state, dispatch] = useReducer(todosReducer, initialState)
```

```js
// src/reducers/todosReducer.js
export const todosReducer = (state, action) => {
  // ADD, DELETE, TOGGLE, FETCH_INIT, FETCH_SUCCESS, FETCH_FAILURE
  switch (action.type) {
    case "ADD":
      return {
        ...state,
        todos: [...state.todos, action.payload],
      }
    case "DELETE":
      return {
        ...state,
        todos: state.todos.filter(el => el.id !== action.payload.id),
      }
    case "TOGGLE":
      return {
        ...state,
        todos: state.todos.map(el => {
          if (el.id === action.payload.id) {
            return {
              ...el,
              isCompleted: !el.isCompleted,
            }
          }
          return el
        }),
      }
    case "FETCH_INIT": // 🆕
      return {
        ...state,
        loading: true,
      }
    case "FETCH_FAILURE": // 🆕
      return {
        ...state,
        loading: false,
        error: action.payload,
      }
    case "FETCH_SUCCESS": // 🆕
      return {
        ...state,
        loading: false,
        error: "",
        todos: action.payload,
      }
    default:
      throw new Error(`Unsupported action type ${action.type} in todosReducer`)
  }
}
```

---

Nous allons maintenant aussi ajouter l'url de l'API en tant qu'une variable d'environment (pour pouvoir la modifier facilement).

```bash
touch .env.local
```

```bash
REACT_APP_API_URL=http://localhost:4000
```

---

## Fetch todos depuis la base de données

```js
// src/components/Todos.js
import { useReducer, useEffect } from "react"
import TodosList from "./TodosList"
import AddTodoForm from "./AddTodoForm"
import { todosReducer } from "../reducers/todosReducer"
import { TodosDispatchContext } from "../context/TodosDispatchContext"

const initialState = {
  todos: [],
  loading: false,
  error: "",
}
const Todos = () => {
  const [state, dispatch] = useReducer(todosReducer, initialState)
  const { todos, loading, error } = state

  useEffect(() => {
    dispatch({ type: "FETCH_INIT" })
    fetch(`${process.env.REACT_APP_API_URL}/todos`)
      .then(response => {
        if (!response.ok) {
          throw new Error(`Something went wrong: ${response.statusText}`)
        }
        return response.json()
      })
      .then(result => {
        dispatch({ type: "FETCH_SUCCESS", payload: result })
      })
      .catch(error => {
        dispatch({ type: "FETCH_FAILURE", payload: error.message })
      })
  }, [isMounted])

  return (
    <main>
      {error && <p className="alert alert-danger">{error}</p>}
      <h2 className="text-center">Ma liste de tâches ({todos.length})</h2>
      <TodosDispatchContext.Provider value={dispatch}>
        <TodosList todos={todos} />
        <AddTodoForm />
        {loading && <p>Loading...</p>}
      </TodosDispatchContext.Provider>
    </main>
  )
}

export default Todos
```

## Custom hook [`useIsMounted`](https://wattenberger.com/blog/react-hooks)

```js
// src/hooks/useIsMounted.js
export const useIsMounted = () => {
  const isMounted = useRef(false)
  useEffect(() => {
    isMounted.current = true
    return () => (isMounted.current = false)
  }, [])
  return isMounted
}
```

```js
const isMounted = useIsMounted()
useEffect(() => {
  dispatch({ type: "FETCH_INIT" })
  fetch(`${process.env.REACT_APP_API_URL}/todos`)
    .then(response => {
      if (!response.ok) {
        throw new Error(`Something went wrong: ${response.statusText}`)
      }
      return response.json()
    })
    .then(result => {
      if (isMounted.current) {
        dispatch({ type: "FETCH_SUCCESS", payload: result })
      }
    })
    .catch(error => {
      if (isMounted.current) {
        dispatch({ type: "FETCH_FAILURE", payload: error.message })
      }
    })
}, [])
```

## Ajouter une tâche

```js
// src/components/AddTodoForm.js
import { useIsMounted } from "../hooks/useIsMounted"
import { useTodosDispatch } from "../context/TodosDispatchContext"

const AddTodoForm = () => {
  const dispatch = useTodosDispatch()
  const isMounted = useIsMounted()

  const handleFormSubmit = event => {
    event.preventDefault()
    const newTodoText = event.target.elements.todo.value
    fetch(`${process.env.REACT_APP_API_URL}/todos`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        text: newTodoText,
        isCompleted: false,
      }),
    })
      .then(response => {
        if (!response.ok) {
          throw new Error(`Something went wrong: ${response.textStatus}`)
        }
        return response.json()
      })
      .then(result => {
        if (isMounted.current) {
          dispatch({ type: "ADD", payload: result })
        }
      })
      .catch(error => {
        if (isMounted.current) {
          dispatch({ type: "FETCH_FAILURE", payload: error.message })
        }
      })
    event.target.reset()
  }
  return // comme avant...
}

export default AddTodoForm
```

## Supprimer une tâche

```js
// src/components/DeleteTodo.js
import { useTodosDispatch } from "../context/TodosDispatchContext"
import { useIsMounted } from "../hooks/useIsMounted"

const DeleteTodo = ({ todo }) => {
  const dispatch = useTodosDispatch()

  const isMounted = useIsMounted()
  const deleteTodo = () => {
    fetch(`${process.env.REACT_APP_API_URL}/todos/${todo.id}`, {
      method: "DELETE",
      headers: {
        "Content-Type": "application/json",
      },
    })
      .then(response => {
        if (!response.ok) {
          throw new Error(`something went wrong ${response.statusText}`)
        }
        return response.json()
      })
      .then(result => {
        if (isMounted.current) {
          dispatch({ type: "DELETE", payload: result })
        }
      })
      .catch(error => {
        if (isMounted.current) {
          dispatch({ type: "FETCH_FAILURE", payload: error.message })
        }
      })
  }
  return (
    <button
      className="btn btn-danger btn-sm"
      type="button"
      onClick={deleteTodo}
    >
      Supprimer
    </button>
  )
}

export default DeleteTodo
```

## Terminer / Rétablir une tâche

```js
// src/components/ToggleTodo.js
import { useTodosDispatch } from "../context/TodosDispatchContext"
import { useIsMounted } from "../hooks/useIsMounted"

const ToggleTodo = ({ todo }) => {
  const dispatch = useTodosDispatch()
  const isMounted = useIsMounted()

  const toggleCompleteTodo = () => {
    fetch(`${process.env.REACT_APP_API_URL}/todos/${todo.id}`, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        ...todo,
        isCompleted: !todo.isCompleted,
      }),
    })
      .then(response => {
        if (!response.ok) {
          throw new Error(`something went wrong ${response.statusText}`)
        }
        return response.json()
      })
      .then(() => {
        if (isMounted.current) {
          dispatch({ type: "TOGGLE", payload: todo })
        }
      })
      .catch(error => {
        if (isMounted.current) {
          dispatch({ type: "FETCH_FAILURE", payload: error.message })
        }
      })
  }
  return (
    <button
      className={`btn btn-sm ${todo.isCompleted ? "btn-dark" : "btn-light"}`}
      type="button"
      onClick={toggleCompleteTodo}
    >
      {todo.isCompleted ? "Rétablir" : "Terminer"}
    </button>
  )
}

export default ToggleTodo
```

## Exercice :

![](https://wptemplates.pehaa.com/assets/alyra.todos.png)
