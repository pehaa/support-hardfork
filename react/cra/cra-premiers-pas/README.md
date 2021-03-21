# CRA

[Create React App (ou CRA)](https://create-react-app.dev/) est un *starter* pour des applications React, officiellement recommandé et maintenu par l'équipe de React.

## Pourquoi ?

- CRA configure plusieurs outils de développement de l'application, en particulier :
  - webpack (bundler des fichiers)
  - babel (compilateur)
  - prettier (pour formater votre code)
  - linter (pour détecter des erreurs)
- CRA lance un serveur de développement en mode _hot reload_ (pas besoin de rafraîchir le navigateur)
- CRA permet de générer la version optimisée pour la _production_

## cra + bootstrap5 step by step

Dans la partie suivante, nous allons démarrer un projet avec CRA et bootstrap 5.

### Step 0 - installer une nouvelle app

```bash
npx create-react-app nom-de-mon-app
cd nom-de-mon-app
code .
```

Nous venons de :

- installer une nouvelle application React, dans un répertoire `nom-de-mon-app`
- nous déplacer dans ce répertoire
- lancer VSCode

### Step 1 - VSCode

Ouvrir le Terminal dans VSCode et lancer app en _mode dévéloppement_

```bash
yarn start
```

L'appli est lancée, elle "hot-reload" sous l'adresse http://localhost:3000 (le numéro du port peut varier). Nous pouvons à chaque moment arrêter le serveur avec `Ctrl+C` (et ensuite le relancer avec `yarn start`).

### Step 2 - bootstrap (si besoin)

Nous allons installer bootstrap5 dans le projet

```bash
yarn add bootstrap@next
```

(Au moment où ce support est rédigé, la version officielle de bootstrap reste encore 4. Une fois version 5 officialisée, nous allons installer bootstap5 avec `yarn add bootstrap`).

Suite à son installation, `bootstrap` sera ajouté dans le dossier `node_modules`.
Un peu plus tard, nous allons **importer** le fichier complet de bootstrap `node_modules/bootstrap/dist/css/bootstrap.css` dans notre application.

### Architecture initiale du projet

```bash
├── README.md
├── node_modules # bootstrap et d'autres librairies externes s'installent ici
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html  # <div id="root"></div> est ici !
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
├── src
│   ├── App.css
│   ├── App.js  # component <App /> est ici !!
│   ├── App.test.js
│   ├── index.css
│   ├── index.js  # ReactDOM.render(<App/ >, document.getElementById("root") se passe ici
│   ├── logo.svg
│   ├── serviceWorker.js
│   └── setupTests.js
└── yarn.lock
```

### public/index.html

C'est un passage obligatoire pour mettre à jour la partie `head` de l'appli (lang, title, meta, favicons, apple-touch-icon, etc.). À l'occasion nous mettons à jour le ficher `public/manifest.json.`

### src/index.js

Ici, on peut mettre en place le fichier CSS principal (fichier bootstrap en l'occurence).

```javascript
import React from "react"
import ReactDOM from "react-dom"
import "bootstrap/dist/css/bootstrap.css" // <- importer bootsrtap, depuis node_modules
import "./index.css" // <- supprimer le contenu par défaut de ce fichier
import App from "./App"
import * as serviceWorker from "./serviceWorker"

// le reste sans changement
```

### Components

Pour mieux organiser notre projet, nous allons créer un dossier _components_ dans le répertoire `src`.
Nous allons ensuite prendre soin de placer tous nos composants dans ce répertoire.

```bash
mkdir src/components
```

Voici comment nous allons mettre en place nos components :

```bash
touch src/components/MonComponent1.js
```

```bash
src
├── App.js
├── App.test.js
├── components
│   ├── MonComponent1.js
│   ├── MonComponent2.js
│   └── MonComponent3.js
├── index.css
├── index.js
├── serviceWorker.js
└── setupTests.js
```

Attention: `MonComponent1` est un très mauvais nom pour un component, nommez vos components selon leurs rôles dans l'application.

#### Component-Starter

Voici un petit starter qui vous permettra de démarrer avec n'importe quel component :

```javascript
// src/components/MonComponent1.js
import React from "react"

const MonComponent1 = () => {
  return null
}

export default MonComponent1
```

### App

Voici l'exemple comment importer un component dans `src/App.js`

```javascript
// src/App.js
import React from "react"
import MonComponent1 from "./components/MonComponent1"

function App() {
  return (
    <div className="container my-3">
      <header className="App-header">
        <h1>Mon titre</h1>
      </header>
      <MonComponent1 />
    </div>
  )
}

export default App
```

---

## Exemple

Vous pouvez voir comment passer le code [depuis ce pen](https://codepen.io/alyra/pen/a4bb96fcc8c2c5dcba3eb2b1720db479) vers une CRA dans [ce répo GitHub.](https://github.com/pehaa/alyra-react-shopping-list)
