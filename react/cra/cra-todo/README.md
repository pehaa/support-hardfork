# Todo List - migration vers CRA

Votre tâche est de migrer la _Todo List_ depuis CodePen vers la solution CRA. Suivez les étapes listées ci-dessus :

## Step 0

```bash
npx create-react-app todos-app
cd todos-app
```

## Step 1 - src/index.js

`index.js` et le fichier de départ de votre application.

- Supprimez le contenu du fichier `src/index.css`
- Installez bootstrap5 dans notre projet et utilisez le fichier css de bootstrap

```bash
yarn add bootstrap@next
```

- Importez le fichier `bootstrap/dist/css/bootstrap.css` depuis le dossier `node_modules/`

```javascript
// src/index.js

import React from "react"
import ReactDOM from "react-dom"
import "bootstrap/dist/css/bootstrap.css"
import App from "./App"
// ... rien ne change ensuite
```

## Step 2 public/index.html

Le fichier HTML qui contient l'élément `<div id="root"></div>` de notre application se trouve dans le répertoire `public`.

C'est dans le fichier `public/index.html` où nous devons modifier l'attribut `lang`, `title` et des attributs `meta`.

## Step 2a - icônes et manifest.json

Le fichier `manifest.json` permet d'installer un bouton vers votre app sur l'appareil mobile, vous pouvez [en lire davantage, par exemple, ici.](https://web.dev/add-manifest-react/)

Vous pouvez créer votre pack d'icônes avec un des générateurs disponibles en ligne, tel que [favicon-generator.org](https://www.favicon-generator.org/).
Copiez ensuite des icônes dans le dossier `public` et remplacez `"short_name"`, `"name"` et tout l'array `"icons"` dans `manifest.json` :

```javascript
{
  "short_name": "Todo App",
  "name": "Liste des tâches - créée avec React",
  "icons": [ ... ]
}
```

## Step 3 - App.js et App.css

Probablement vous n'avez pas besoin d'un fichier .css global, autre que `bootstrap/dist/css/bootstrap.css`.  
Supprimez le fichier `App.css` et ne l'importez pas.
À la fin de cette étape votre application devrait afficher le titre _ToDos App_ :

```javascript
/* src/App.js */
import React from "react"

function App() {
  return (
    <div className="container my-4">
      <h1 className="text-center">ToDos App</h1>
    </div>
  )
}

export default App
```

## Step 4 components folder

- Placez tous les components dans un nouveau dossier `components`.

```bash
mkdir src/components
```

- Respectez cette structure dans votre dossier `components` :

```bash
src
├── App.js
├── App.test.js
├── components
│   ├── AddTodoForm.js
│   ├── Todo.js
│   └── Todos.js
├── index.css
├── index.js
├── serviceWorker.js
└── setupTests.js
```

## Step 5 Components et leurs dépendances

`Todos` utilise :

- `Todo`
- `AddTodoForm`
- la bibliothèque externe _uuid_, que vous devez installer

```bash
yarn add uuid
```

```javascript
// src/components/Todos.js
import React, { useState } from "react"
import Todo from "./Todo"
import AddTodoForm from "./AddTodoForm"
import { v4 as uuidv4 } from "uuid"

const Todos = () => {
  return null
}

export default Todos
```

```javascript
// src/components/Todo.js
import React from "react"

const Todo = () => {
  return null
}

export default Todo
```

```javascript
// src/components/AddTodoForm.js
import React from "react"

const AddTodoForm = () => {
  return null
}

export default AddTodoForm
```

### App.js

```javascript
// src/App.js
import React from "react"
import Todos from "./components/Todos"

function App() {
  return (
    <div className="container my-4">
      <h1 className="text-center">ToDos App</h1>
      <Todos />
    </div>
  )
}

export default App
```

### Todos.js

- Avec `import React, { useState } from "react"` importez `useState` directement. Vous allez pouvoir utiliser `useState` au lieu de `React.useState`.
- `v4` est un _named export_ depuis `uuid`, et nous allons le renommer en `uuidv4`
  renommer
  Voici comment mettre on place `Todos.js`

```javascript
import { v4 as uuidv4 } from "uuid"
```

```javascript
// src/components/Todos.js
import React, { useState } from "react"
import Todo from "./Todo"
import AddTodoForm from "./AddTodoForm"
import { v4 as uuidv4 } from "uuid"

const Todos = () => {
  const [todos, setTodos] = useState([])
  const addTodo = (text) => {
    const newTodo = {
      text,
      isCompleted: false,
      id: uuidv4(),
    }
    console.log(newTodo)
    setTodos([...todos, newTodo])
  }

  const deleteTodo = (task) => {
    setTodos(todos.filter((el) => el !== task))
  }

  const toggleCompleteTodo = (task) => {
    setTodos(
      todos.map((el) => {
        return {
          ...el,
          isCompleted: task.id === el.id ? !el.isCompleted : el.isCompleted,
        }
      })
    )
  }

  const completedCount = todos.filter((el) => el.isCompleted).length
  return (
    <>
      <h2 className="text-center">
        Ma liste de tâches ({completedCount} / {todos.length})
      </h2>
      {todos.map((el) => {
        return (
          <Todo
            key={el.id}
            todo={el}
            deleteTodo={deleteTodo}
            toggleCompleteTodo={toggleCompleteTodo}
          />
        )
      })}
      <AddTodoForm addTodo={addTodo} />
    </>
  )
}

export default Todos
```

### Working App

Complétez `Todo` et `AddTodoForm`

## Step 6 Déploiement sur netlify

Pushez votre app sur votre repo GitHub et déployer avec netlify 🚀.
