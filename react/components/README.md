# Components

**Components** (ou composants en français) constitue une base de React. Ils permettent de découper l’interface utilisateur en éléments indépendants et réutilisables. Nous créons nos propres briques avec lesquelles nous assemblons notre application web. Ceci nous permet de travailler plus vite, ne pas nous répéter et mieux organiser notre code. Il est plus facile de modifier une partie spécifique de markup, et de réutiliser la même interface pour différents contenus.

## React - recap

Nous venons d'apprendre que :

1. Nous pouvons intéragir avec DOM (Document Object Model) via JavaScript. Autremement dit, nous pouvons créer des interfaces utilisateurs dans le navigateur avec JavaScript. Ceci est possible avec des méthodes d'API DOM telles que par exemple :

   - `document.createElement` (pour créer)
   - `document.querySelector` ou `document.getElementById` (pour sélectionner)
   - `el.append` (pour insérer)

---

2. Il existe des libraries JavaScript (dont la plus importante ✨React✨) qui sont faites pour nous faciliter ceci.

   ***

3. L'application React "vit" dans un élément du DOM, `<div id="root"></div>`.

   ***

4. L'application React est un objet des objets, une structure d'arborescence, un _tree_ que nous mettons en place de façon déclarative grâce à la syntaxe JSX.

   ***

5. React prend soins d'intégrer ce _tree_ dans le DOM.

   ***

6. Quand l'état de notre application change, React compare l'état de son _tree_ avant et après et ensuite met à jour le DOM, agissons ponctuellement sur les éléments qui ont changé.

   ***

7. Nous avons appris comment créer des éléments React. Nous avons vu qu'un élément React peut avoir des descendants (`children`).

   ***

8. Dans les exercices de la partie JSX-2 nous avons utilisé des fonctions pour créer des éléments React. Nous somment alors à un pas de la notion des **components**.

## Component step by step

Imaginons que nous souhaitions mettre en place un catalogue des écoles. Nous commençons à réunir des données :

```javascript
const Alyra = {
  name: "Alyra",
  link: "https://alyra.fr",
  description:
    "Une école au coeur de la blockchain. Fondée par des passionnés et ouverte à toutes et tous.",
}

const Simplon = {
  name: "Simplon",
  link: "https://simplon.co",
  description:
    "Un réseau de Fabriques solidaires et inclusives qui proposent des formations gratuites aux métiers techniques du numérique.",
}
```

https://codepen.io/alyra/pen/xxgOXoz

Voici comment nous souhaitons afficher l'information sur Alyra :

```javascript
const schoolAlyra = (
  <article className="p-3 mb-3 border shadow">
    <h2 className="text-center">Alyra</h2>
    <p>
      Une école au coeur de la blockchain. Fondée par des passionnés et ouverte
      à toutes et tous.
    </p>
    <a href="https://alyra.fr" className="btn btn-primary btn-sm">
      En savoir plus sur Alyra
    </a>
  </article>
)
```

ce serait pareil pour Simplon :

```javascript
const schoolSimplon = (
  <article className="p-3 mb-3 border shadow">
    <h2 className="text-center">Simplon</h2>
    <p>
      Un réseau de Fabriques solidaires et inclusives qui proposent des
      formations gratuites aux métiers techniques du numérique.
    </p>
    <a href="https://simplon.co" className="btn btn-primary btn-sm">
      En savoir plus sur Simplon
    </a>
  </article>
)
```

ainsi que pour chaque nouvelle école que nous ajouterons sur la liste.

Afin d'afficher les écoles dans un document HTML nous créons un `element` comme ceci :

```javascript
const element = (
  <section className="container">
    <h1 className="my-3 text-center">Ma liste des écoles à recommander</h1>
    <div className="row">
      <div className="col-md-6">{schoolAlyra}</div>
      <div className="col-md-6">{schoolSimplon}</div>
    </div>
  </section>
)

ReactDOM.render(element, document.getElementById("root"))
```

`schoolAlyra` et `schoolSimplon` (ainsi que d'autres écoles que j'ajouterai dans l'avenir) partagent le même markup avec des différences dans le nom, la description et le lien vers l'école.

Nous pouvons créer une fonction qui prend comme paramètres le nom, le lien et la décription et retourne notre article. Voici à quoi auraient pu ressembler une telle fonction et son application :

```javascript
const school = (name, link, description) => {
  return (
    <article className="p-3 mb-3 border shadow">
      <h2 className="text-center">{name}</h2>
      <p>{description}</p>
      <a href={link} className="btn btn-primary btn-sm">
        En savoir plus sur {name}
      </a>
    </article>
  )
}

const element = (
  <section className="container">
    <h1 className="my-3 text-center">Ma liste des écoles à recommander</h1>
    <div className="row">
      <div className="col-md-6">
        {school("Alyra", "https://alyra.fr", "Une école au coeur de la blockchain...")}
      </div>
      <div className="col-md-6">
        {school("Simplon", "https://simplon.com", "Un réseau de Fabriques solidaires....")}
      </div>
  </section>
)

ReactDOM.render(element, document.getElementById("root"))
```

Mais, nous n'allons pas faire ceci. Nous allons créer notre premier **component** React.

**Component React est une fonction**

- qui prend comme paramètre un objet (par convention nommé toujours `props`)
- dont le nom commence toujours par une lettre en majuscule
- qui retourne un élément React (un component peur retourner aussi un `string`, `number`, `boolean`, `null`, un Array des ceci.)

Nous allons alors créer un component `School`

```javascript
const School = (props) => {
  const { name, link, description } = props
  return (
    <article className="p-3 mb-3 border shadow">
      <h2 className="text-center">{name}</h2>
      <p>{description}</p>
      <a href={link} className="btn btn-primary btn-sm">
        En savoir plus sur {name}
      </a>
    </article>
  )
}
```

ensuite nous pouvons l'appeler comme ceci `School({name: "Alyra", link: "https://alyra.fr", description: "Une école au coeur de la blockchain..."})` ou **plutôt comme ci-dessous** ✨ :

```javascript
const element = (
  <section className="container">
    <h1 className="my-3 text-center">Ma liste des écoles à recommander</h1>
    <div className="row">
      <div className="col-md-6">
        <School
          name="Alyra"
          link="https://alyra.fr"
          description="Une école au coeur de la blockchain"
        />
      </div>
      <div className="col-md-6">
        <School
          name="Simplon"
          link="https://simplon.co"
          description="Un réseau de Fabriques solidaires et inclusives..."
        />
      </div>
    </div>
  </section>
)

ReactDOM.render(element, document.getElementById("root"))
```

**Attention** les noms des components **doivent** commencer par une lettre majuscule, `<school />` serait interprétée par React comme un HTML tag `school`.

### Component et props

`props` vient de propriétés, `props` est un objet qui regroupe toutes les propriétés de notre component.
Il ne faut **jamais** modifier `props` dans le component.

#### props.children

Que se passe-t'il si nous appelons `<School name="Alyra" link=".." description="..">tu me vois ?</School>` avec un élément enfant (ici texte "tu me vois ?")

https://codepen.io/alyra/pen/wvGzVwN

Le rendu ne change pas, mais regardons les `props` dans la console, props contient la clé children qui correspond à notre "tu me vois ?"

![](https://assets.codepen.io/4515922/props-children.png)

Dans notre component, nous avons accès à ses éléments enfants. Ceci nous permet de définir notre component un peu différemment, avec plus de flexibilité.

```javascript
const School = (props) => {
  const { name, link, children } = props
  return (
    <article className="p-3 mb-3 border shadow">
      <h2 className="text-center">{name}</h2>
      {children}
      <a href={link} className="btn btn-primary btn-sm">
        En savoir plus sur {name}
      </a>
    </article>
  )
}

const element = (
  <section className="container">
    <h1 className="my-3 text-center">Ma liste des écoles à recommander</h1>
    <School name="Alyra" link="https://alyra.fr">
      <p>Une école au coeur de la blockchain...</p>
    </School>
    <School name="Simplon" link="https://simplon.co">
      <p>Un réseau de Fabriques solidaires et inclusives...</p>
    </School>
  </section>
)

ReactDOM.render(element, document.getElementById("root"))
```

Avec cette approche nous pouvons inclure dans notre description des balises HTML ou imbriquer d'autres components React.

https://codepen.io/alyra/pen/yLgJzja

## Nesting components

```javascript
const School = (props) => {
  const { name, link, children } = props
  return (
    <article className="p-3 mb-3 border shadow">
      <h2 className="text-center">{name}</h2>
      {children}
      <SchoolLink name={name} link={link} />
    </article>
  )
}

const SchoolLink = (props) => {
  return (
    props.link && (
      <a
        href={props.link}
        className="btn btn-primary btn-sm"
        target="_blank"
        rel="noopener noreferrer"
      >
        En savoir plus sur {props.name}
      </a>
    )
  )
}

const App = () => (
  <section className="container">
    <h1 className="my-3 text-center">Ma liste des écoles à recommander</h1>
    <School name="Alyra" link="https://alyra.fr">
      <p>
        Une école au coeur de <b>la blockchain</b>...
      </p>
    </School>
    <School name="Simplon" link="https://simplon.co">
      <p>Un réseau de Fabriques solidaires et inclusives...</p>
    </School>
  </section>
)

ReactDOM.render(<App />, document.getElementById("root"))
```

ou comme ceci :

```javascript
const School = (props) => {
  const { name, children } = props
  return (
    <article className="p-3 mb-3 border shadow">
      <h2 className="text-center">{name}</h2>
      {children}
    </article>
  )
}

const SchoolLink = (props) => {
  const { name, link } = props
  return (
    link && (
      <a
        href={props.link}
        className="btn btn-primary btn-sm"
        target="_blank"
        rel="noopener noreferrer"
      >
        En savoir plus sur {name}
      </a>
    )
  )
}

const App = () => (
  <section className="container">
    <h1 className="my-3 text-center">Ma liste des écoles à recommander</h1>
    <School name="Alyra" link="https://alyra.fr">
      <p>
        Une école au coeur de <b>la blockchain</b>...
      </p>
      <SchoolLink name="Alyra" link="https://alyra.fr" />
    </School>
    <School name="Simplon" link="https://simplon.co">
      <p>Un réseau de Fabriques solidaires et inclusives...</p>
      <SchoolLink name="Simplon" link="https://simplon.co" />
    </School>
  </section>
)

ReactDOM.render(<App />, document.getElementById("root"))
```

https://codepen.io/alyra/pen/LYxZzdm

## Décomposition au niveau d'argument

Parfois, on utilise la décomposition au niveau des arguments d'un component, comme ceci :

```javascript
const School = ({ name, link, children }) => {
  return (
    <article className="p-3 mb-3 border shadow">
      <h2 className="text-center">{name}</h2>
      {children}
      <SchoolLink name={name} link={link} />
    </article>
  )
}

const SchoolLink = ({ name, link }) => {
  return (
    !!link && (
      <a
        href={props.link}
        className="btn btn-primary btn-sm"
        target="_blank"
        rel="noopener noreferrer"
      >
        En savoir plus sur {name}
      </a>
    )
  )
}
```

## Décomposition avec une valeur par défaut

Il est aussi possible de mettre en place **une valeur par défaut** pour une des props

```javascript
const Button = (props) => {
  const {type = "button", children} = props
  return (<button type={type} className="btn">{children})</button>)
}

<Button>Click me</Button> // <button type="button" class="btn">Click me</button>
<Button type="submit">Send</Button> // <button type="submit" class="btn">Send</button>
```

---

## Exercices

- [Components Welcome](https://codepen.io/alyra/pen/yLOzzpw) | [solution](https://codepen.io/alyra/pen/b1c47bb3d5f57cdc71846e81e04a3417)
- [Components 404](https://codepen.io/alyra/pen/LYNzaEy) | [solution](https://codepen.io/alyra/pen/37312356f794026ae368fd81d2d056e4)
- [Components - conditional - dashboard](https://codepen.io/alyra/pen/wvGrPRv) | [solution](https://codepen.io/alyra/pen/f453f82222b1bc2548d5fa9426f87182)
- [Components - conditional -admin](https://codepen.io/alyra/pen/VwaMrPX) | [solution](https://codepen.io/alyra/pen/397a299a010c3ab389905b3ca3b24e85)
- [Components - conditional - notifications](https://codepen.io/alyra/pen/VwaMyWJ) | [solution](https://codepen.io/alyra/pen/0c661571511cf77eda61a36eea6cc666)
- [Components - button](https://codepen.io/alyra/pen/WNwZmjq) | [solution](https://codepen.io/alyra/pen/202e3fce09a39d4835e6d81b4f9e40c4)
- [Components - Gradients](https://codepen.io/alyra/pen/qBZqjmb) | [solution](https://codepen.io/alyra/pen/fc101924ee4348e9a009d295e80b266b)

--

- [Component Card](https://codepen.io/alyra/pen/YzqEWjv) | [solution](https://codepen.io/alyra/pen/fdff1bc181fc6847092b07fe1ab1a309)
- [Components - LabelTextInput](https://codepen.io/alyra/pen/ExKbyjK) | [solution](https://codepen.io/alyra/pen/3fe809fdf569707c7041ef12e9c44d93)
- [Components - Gradients + GradientsList](https://codepen.io/alyra/pen/GRZOJBM) | [solution](https://codepen.io/alyra/pen/58bf816e5cdc601f6aedf09b5a6db992)
