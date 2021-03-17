# Custom Bootstrap5 CSS build

## Prérequis

**`node` est installé.**

Vous allez utiliser votre repo Alyra Mag (challenge bootstrap) en tant que le point de départ. Au lieu de charger le fichier bootstrap depuis un _CDN_, nous allons utiliser le _package manager_ [npm](https://www.youtube.com/watch?v=ZNbFagCBlwo) pour installer bootstrap.
_npm_ téléchargera et mettra dans notre projet les fichiers bootstrap.

---

## `package.json`

Nous allons configurer le fichier `package.json` avec `npm`.  
`package.json` est un fichier en format `JSON` (nous allons parler davantage de ce format dans le contexte du JavaScript un peu plus tard).  
Le fichier `package.json` "habite" dans la racine d'un projet et contient des métadonnées pertinentes pour le projet, les informations sur l'auteur du projet, la licence, mais aussi **des dépendances (des bibliothèques externes qui doivent être installées).**

Afin de créer `package.json` et entamer sa configuration nous lançons la commande `npm init` dans le terminal :

```bash
npm init
```

La ligne de commande posera quelques questions, les réponses seront intégrées dans le fichier `package.json`, créé automatiquement.

Pour l'instant les valeurs par défaut sont tout à fait ok, pour ceci vous pouvez ajouter l'option `--yes` :

```bash
npm init --yes
```

---

## `.gitignore`

Avant que nous passions à l'installation du bootstrap depuis _npm_, on devrait s'assurer d'avoir `node_modules` dans le fichier `.gitignore`.  
Le fichier caché `.gitignore` spécifie les fichiers et/ou répertoires qui ne devraient pas être traqués par git.

Si votre projet ne contient pas de fichier `.gitignore` vous pouvez le créer dans la racine du projet

```bash
touch .gitignore
```

Voici un exemple de son contenu

```
.DS_Store
node_modules
```

---

## Installation

Passons maintenant à l'installation du bootstrap.
Au moment de l'écriture de ce document, "5" que nous utilisons, n'est pas encore la version officielle, pour cela nous ajoutons `@next` dans la commande de l'installation :

```bash
npm install bootstrap@next
```

Et quand bootstrap5 deviendra la version officielle, ce sera simplement :

```bash
npm install bootstrap
```

Une fois l'installation terminée, de nouveaux dossiers et fichiers apparaissent 💫

```bash
├── node_modules
│   ├── bootstrap
│   │   ├── LICENSE
│   │   ├── README.md
│   │   ├── dist
│   │   │   ├── css
│   │   │   └── js
│   │   ├── js
│   │   │   ├── dist
│   │   │   └── src
│   │   ├── package.json
│   │   └── scss
│   │       ├── _alert.scss
│   │       ├── _badge.scss
│   │       ├── ...
│   │       ├── _variables.scss
│   │       ├── bootstrap-grid.scss
│   │       ├── bootstrap-reboot.scss
│   │       ├── bootstrap-utilities.scss
│   │       ├── bootstrap.scss
│   │       ├── forms
│   │       ├── helpers
│   │       ├── mixins
│   │       ├── utilities
│   │       └── vendor
```

L'information sur les dépendances installées vient d'être ajoutée dans le fichier `package.json`. Maintenant votre projet **sait** de quoi il a besoin. Si vous supprimez le dossier `node_modules`, il suffit de taper dans le terminal

```bash
npm install
```

et tout ce dont votre projet a besoin sera re-installé.

---

## Dossier `custom`

Créer un dossier `custom` dans la racine du projet `alyra-challenge-custom-b5`

```bash
mkdir custom
```

Dans le dossier `custom` créer le fichier `mybootstrap.scss`

```bash
touch custom/mybootstrap.scss
```

---

## Configuration de Sass Live Compiler

Nous allons installer et configurer l'extenstion VSCode, **Sass Live Compiler**

![](https://wptemplates.pehaa.com/assets/alyra/sasscompiler.png)

- créer un dossier `.vscode` (dans la racine du projet `alyra-challenge-custom-b5` )
- dedans créer un fichier `settings.json`
- reprendre l'exemple de config depuis [la FAQ de plugin Live Sass Compiler](https://ritwickdey.github.io/vscode-live-sass-compiler/docs/faqs.html)
- notez que `node_modules` où se trouvent fichiers scss de bootstrap sont exclus, et c'est tout à fait souhaité

```
{
     "liveSassCompile.settings.formats":[
        {
            "format": "expanded",
            "extensionName": ".css",
            "savePath": "/css"
        },
        {
            "extensionName": ".min.css",
            "format": "compressed",
            "savePath": "/dist/css"
        }
    ],
    "liveSassCompile.settings.excludeList": [
       "**/node_modules/**",
       ".vscode/**"
    ],
    "liveSassCompile.settings.generateMap": true,
    "liveSassCompile.settings.autoprefix": [
        "> 1%",
        "last 2 versions"
    ]
}
```

---

## Watch Sass

Lancer `Watch Sass` pour vérifier la configuration.

```scss
// custom/mybootstrap.scss
body {
  color: red;
}
```

On devrait apercevoir de nouveaux dossiers et fichiers comme dans l'arborescence ci-dessous

```bash
├── README.md
├── blog-single.html
├── category.html
├── contact.html
├── css
│   ├── mybootstrap.css
│   └── mybootstrap.css.map
├── custom
│   └── mybootstrap.scss
├── dist
│   └── css
│       ├── mybootstrap.min.css
│       └── mybootstrap.min.css.map
├── images
├── node_modules
├── index.html
├── package.json
├── screenshots
└── style.css
```

## Bootstrap depuis le fichier local

Dans les fichiers `.html` remplacez le lien vers le CSS de bootstrap (CDN) par le lien vers `dist/css/mybootstrap.min.css`

avant :

```html
<link
  href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css"
  rel="stylesheet"
  integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl"
  crossorigin="anonymous"
/>
```

après :

```html
<link rel="stylesheet" href="dist/css/mybootstrap.min.css" />
```

---

## Personnalisation

D'une manière générale, **il n'est pas une bonne pratique de modifier les fichiers sources.** On ne touchera **jamais** aux fichiers dans le dossier `node_modules.` ☠️

Le fichier principal scss de bootstrap est le `bootstrap.scss` dans le dossier `node_modules/bootstrap/scss/` Au lieu de le modifier, nous allons copier son contenu (le contenu du fichier `node_modules/bootstrap/scss/bootstrap.scss`) et le coller dans notre fichier `custom/mybootstrap.scss`

Maintenant nous devons corriger les chemins vers les fichiers partials :

```scss
// custom/mybootstrap.scss
@import "../node_modules/bootstrap/scss/functions";
@import "../node_modules/bootstrap/scss/variables";
@import "../node_modules/bootstrap/scss/mixins";
@import "../node_modules/bootstrap/scss/utilities";
...
```

Astuce: vous pouvez vous servir de la fonctionnalité "Replace" de VSCode Alt+Cmd/Ctrl+F

cherche : `@import "`  
replace with : `@import "../node_modules/bootstrap/scss/`

![](https://wptemplates.pehaa.com/assets/alyra/replace-nm.png)

---

### Sélection des partials

Commentez les partials dont vous vous ne servez pas.

```scss
// custom/mybootstrap.scss
...
// @import "../node_modules/bootstrap/scss/toasts";
// @import "../node_modules/bootstrap/scss/modal";
// @import "../node_modules/bootstrap/scss/tooltip";
// @import "../node_modules/bootstrap/scss/popover";
@import "../node_modules/bootstrap/scss/scss/carousel";
// @import "../node_modules/bootstrap/scss/spinners";
...

```

### Variables

> Regardez ces 3 exemples. Quelle couleur prendra body ? (Vous pouvez confirmer vos intuitions directement dans [SassMeister](https://www.sassmeister.com/))

**Exemple 1 :**

```scss
$brand-color: orange;
$brand-color: red;

body {
  background: $brand-color;
}
```

**Exemple 2 :**

```scss
$brand-color: orange;
$brand-color: red !default;

body {
  background: $brand-color;
}
```

**Exemple 3 :**

```scss
$brand-color: orange;
$brand-color: red !default;
$brand-color: tomato;

body {
  background: $brand-color;
}
```

Quel est l'impact de `!default` dans la définition d'une variable. C'est très pratique, et souvent utilisé par des auteurs des bibliothèques. La valeur avec le flag `!default` va être utilisé uniquement si la variable n'est pas mise en place ailleurs (soit après, mais aussi avant).

**Exemple 1 :**

```scss
$brand-color: orange;
// red écrase orange
$brand-color: red;

body {
  background: $brand-color;
}
```

**Exemple 2 :**

```scss
$brand-color: orange;
// red ne sera pas utilisé, $brand-color a pris la couleur orange
$brand-color: red !default;

body {
  background: $brand-color;
}
```

**Exemple 3 :**

```scss
$brand-color: orange;
// red ne sera pas utilisé, $color a pris la couleur orange
$brand-color: red !default;
// tomato écrase orange
$brand-color: tomato;

body {
  background: $brand-color;
}
```

---

### Personnalisation des variables

En haut du fichier `mybootstrap.scss` ajoutez

```scss
$secondary: indigo;
$dark: black;
$light: lavender;
$danger: hotpink;
$box-shadow: 0 2px 10px lavender;
```
