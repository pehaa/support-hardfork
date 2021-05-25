# JWT Authentication

## Front-end

Vous pouvez continuer avec votre repo ou démarrer avec [celui-ci](https://github.com/pehaa/alyra-todos-jwt) (dans ce cas n'oubliez pas de mettre en place `.env.local`).

```bash
cd alyra-todos-jwt
yarn
touch .env.local
```

```bash
# .env.local
REACT_APP_API_URL=http://localhost:4000
```

```bash
yarn start
```

## Mise en place du backend

Clonez [ce repo](https://github.com/pehaa/fake-api-auth)

```bash
cd fake-api-auth
yarn
yarn start
```

Nous allons utiliser l'authentication avec JSON webtoken :

![](https://wptemplates.pehaa.com/assets/alyra/jwt.png)

### `POST /auth/login` - avec `fetch`

```js
fetch("http://localhost:4000/auth/login", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    login: "admin",
    password: "alyra",
  }),
})
```

![](https://wptemplates.pehaa.com/assets/alyra/jwt-response.png)

### `GET` - avec `fetch`

```js
fetch("http://localhost:4000/todos", {
  headers: {
    "Content-Type": "application/json",
    Authorization: "Bearer " + user.access_token,
  },
})
```

### `POST` - avec `fetch`

```js
fetch("http://localhost:4000/todos", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    Authorization: "Bearer " + user.access_token,
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
    Authorization: "Bearer " + user.access_token,
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
    Authorization: "Bearer " + user.access_token,
  },
})
```

## user

![](https://wptemplates.pehaa.com/assets/alyra/todostree.png)

![](https://wptemplates.pehaa.com/assets/alyra/usercontext.png)

```bash
touch src/context/UserContext.js
touch src/reducers/userReducer.js
```

```js
// reducers/userReducer.js
// actions: LOGIN_INIT, LOGIN_SUCCESS, LOGIN_FAILURE, LOGOUT

export const userReducer = (state, action) => {
  switch (action.type) {
    case "LOGIN_INIT":
      return {
        ...state,
        loading: true,
        error: "",
      }
    case "LOGIN_FAILURE":
      return {
        ...state,
        user: null,
        loading: false,
        error: action.payload,
      }
    case "LOGIN_SUCCESS":
      return {
        ...state,
        user: action.payload,
        loading: false,
        error: "",
      }
    case "LOGOUT":
      return {
        ...state,
        user: null,
        loading: false,
        error: "",
      }
    default:
      throw new Error(`Unsupported action type ${action.type} in userReducer`)
  }
}
```

```js
// src/context/UserContext.js
import { createContext, useContext, useReducer, useEffect } from "react"
import { userReducer } from "../reducers/userReducer"

const UserContext = createContext()

export const UserContextProvider = ({ children }) => {
  const [state, userDispatch] = useReducer(userReducer, {
    user: null,
    loading: false,
    error: false,
  })
  const { user, loading, error } = state
  return (
    <UserContext.Provider value={{ user, loading, error, userDispatch }}>
      {children}
    </UserContext.Provider>
  )
}

export const useUser = () => {
  const context = useContext(UserContext)
  if (context === undefined) {
    throw new Error(
      `You should use useUser only within the UserContext.Provider`
    )
  }
  return context
}
```

Nous allons mettre en place notre UserContextProvider, il va enveloper tout notre application.

```js
// src/index.js
// comme avant
import { UserContextProvider } from "./context/UserContext"

ReactDOM.render(
  <React.StrictMode>
    <UserContextProvider>
      <App />
    </UserContextProvider>
  </React.StrictMode>,
  document.getElementById("root")
)
// comme avant
```

Et finalement dans notre component `Login.js`

```js
// src/components/Login.js
import { useHistory } from "react-router-dom"
import { useUser } from "../context/UserContext"
import { useIsMounted } from "../hooks/useIsMounted"

const Login = () => {
  const history = useHistory()
  const { error, loading, userDispatch } = useUser()
  const isMounted = useIsMounted()
  const handleFormSubmit = e => {
    e.preventDefault()
    const login = e.target.elements.login.value
    const password = e.target.elements.password.value
    userDispatch({
      type: "LOGIN_INIT",
    })
    fetch(`${process.env.REACT_APP_API_URL}/auth/login`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        login,
        password,
      }),
    })
      .then(response => {
        if (!response.ok) {
          throw new Error(`Erreur, ${response.statusText}`)
        }
        return response.json()
      })
      .then(result => {
        if (isMounted.current) {
          userDispatch({
            type: "LOGIN_SUCCESS",
            payload: result,
          })
          // si ok nous allons rediriger l'utilisateur vers "todos"
          history.replace("/todos")
        }
      })
      .catch(e => {
        if (isMounted.current) {
          userDispatch({
            type: "LOGIN_FAILURE",
            payload: e.message,
          })
        }
      })
  }
  return (
    <section>
      <h2 className="text-center">Log in</h2>
      {loading ? (
        <p>loading...</p>
      ) : (
        <form className="row g-3 mt-3" onSubmit={handleFormSubmit}>
          <div className="col-auto">
            <label htmlFor="login" className="visually-hidden">
              Login
            </label>
            <input
              type="text"
              className="form-control"
              id="login"
              placeholder="login"
              required
            />
          </div>
          <div className="col-auto">
            <label htmlFor="password" className="visually-hidden">
              Password
            </label>
            <input
              type="password"
              className="form-control"
              id="password"
              placeholder="Password"
              required
            />
          </div>
          <div className="col-auto">
            <button type="submit" className="btn btn-primary mb-3">
              Log in
            </button>
          </div>
        </form>
      )}
      {error && <p className="alert alert-danger">{error}</p>}
    </section>
  )
}

export default Login
```

## Routes pour l'utilisateur authentifié et non-authentifié

### Navigation

L'utilisateur non-authentifié 😎 voit dans la navigation :

- Home
- Todos (qui redirige vers Log in (\*))
- Log in

L'utilisateur authentifié 🤓 voit dans la navigation :

- Home
- Todos
- Log out (ceci n'est pas un `Link` mais un bouton qui dispatch `LOGOUT`)

Nous allons modifier notre component `Navigation`

**avant :**

```js
import { Link } from "react-router-dom"

const Navigation = () => {
  return (
    <nav>
      <ul className="nav">
        <li className="nav-item">
          <Link className="nav-link" to="/">
            Home 🏡
          </Link>
        </li>
        <li className="nav-item">
          <Link className="nav-link" to="/todos">
            Todos 📋
          </Link>
        </li>
        <li className="nav-item">
          <Link className="nav-link" to="/login">
            Log in 🔓
          </Link>
        </li>
        <li className="nav-item">
          <button>Log out 🔐</button>
        </li>
      </ul>
    </nav>
  )
}

export default Navigation
```

**après :**

```js
import { Link } from "react-router-dom"
import { useUser } from "../context/UserContext"

const Navigation = () => {
  const { user, userDispatch } = useUser()
  console.log(user)
  return (
    <nav className="container">
      <ul className="nav">
        <li className="nav-item">
          <Link className="nav-link" to="/">
            Home 🏡
          </Link>
        </li>
        <li className="nav-item">
          <Link className="nav-link" to="/todos">
            Todos 📋
          </Link>
        </li>
        {!user ? (
          <li className="nav-item">
            <Link className="nav-link" to="/login">
              Log in 🔓
            </Link>
          </li>
        ) : (
          <li className="nav-item ms-auto">
            <button
              className="nav-link btn btn-primary text-light"
              onClick={() => {
                userDispatch({ type: "LOGOUT" })
              }}
            >
              Log out 🔐
            </button>
          </li>
        )}
      </ul>
    </nav>
  )
}

export default Navigation
```

### (\*) Todos _private_ route

Nous allons mettre en place un _private_ route, '/todos' sera accessible uniquement pour l'utilisateur authentifié. Dans le cas contraire l'utilisateur est redirigé vers la page `/login`

```js
// src/App.js

/*
// avant
<Route exact path="/todos">
  <Todos />
</Route>
*/

<Route exact path="/todos">
  {user ? <Todos /> : <Redirect to="/login" />}
</Route>
```

## Persistent user

Que se passe-t-il quand l'utilisateur recharge la page ? Les données sont effacées et il faut à chaque fois se reconnecter à nouveau.

La solution la plus simple pour y remédier est de se servir de localStorage du navigateur (attention - cette méthode n'est pas idéale, car localStorage peut être compromis suite à un attaque XSS).

```js
export const UserContextProvider = ({ children }) => {
  const [state, dispatch] = useReducer(
    userReducer,
    {
      user: null,
      loading: false,
      error: false,
    },
    initialState => {
      return {
        ...initialState,
        user: JSON.parse(localStorage.getItem("authUser")) || null,
      }
    }
  )

  const { user } = state

  useEffect(() => {
    localStorage.setItem("authUser", JSON.stringify(user))
  }, [user])
  return (
    <UserContext.Provider value={{ state, dispatch }}>
      {children}
    </UserContext.Provider>
  )
}
```

## Fetching /todos

Dans `Todos.js`, `AddTodoForm.js`, `DeleteTodo.js` et `ToggleTodo.js` :

```js
const { user } = useUser()

fetch(/* */, {
  // comme avant
  headers: {
   // comme avant
    Authorization: "Bearer " + user.access_token, // 🆕
  }
  // comme avant
})
```

## Expiration du `access_token`

`access_token` a une vie limitée ⏳.  
Dans notre cas, `access_token` expire toute les 60 minutes.  
Ceci avec le `localStorage` peut provoquer des situation imprévu comme ceci :

![](https://wptemplates.pehaa.com/assets/alyra/expired.png)

Dans `Todos.js`, `AddTodoForm.js`, `DeleteTodo.js` et `ToggleTodo.js` :

```js
const { user, dispatchUser } = useUser()

fetch(/* */, {
  // comme avant
  headers: {
   // comme avant
    Authorization: "Bearer " + user.access_token,
  }
  // comme avant
})
.then((response) => {
  if (response.status === 401) {
    userDispatch({ type: "LOGOUT" })
  }
  if (!response.ok) {
    throw new Error(`Something went wrong: ${response.statusText}`)
  }
  return response.json()
})
```
