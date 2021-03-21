# Components

**Components** (ou composants en français) constitue une base de React. Ils permettent de découper l’interface utilisateur en éléments indépendants et réutilisables. Nous créons nos propres briques avec lesquelles nous assemblons notre application web. Ceci nous permet de travailler plus vite, ne pas nous répéter et mieux organiser notre code. Il est plus facile de modifier une partie spécifique de markup, réutiliser la même interface pour différents contenus est aussi quasi automatique.

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
    {schoolAlyra}
    {schoolSimplon}
  </section>
)

ReactDOM.render(element, document.getElementById("root"))
```

`schoolAlyra` et `schoolSimplon` (ainsi que d'autres écoles que j'ajouterai dans l'avenir) partagent le même markup avec des différences dans le nom, la description et le lien vers l'école.

Pour ne pas me répéter (le principe _DRY = don't repeat yourself_), nous pouvons créer une fonction qui retourne notre article. Voici à quoi auraient pu ressembler une telle fonction et son application :

```javascript
const school = (props) => {
  /* 
  props = {
    name: "Alyra",
    link: "https://alyra.fr",
    description:
    "Une école au coeur de la blockchain. Fondée par des passionnés et ouverte à toutes et tous.",
  } 
  */
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

const element = (
  <section className="container">
    <h1 className="my-3 text-center">Ma liste des écoles à recommander</h1>
    {school(Alyra)}
    {school(Simplon)}
  </section>
)

ReactDOM.render(element, document.getElementById("root"))
```

On y est presque. Avec React nous pouvons aller plus loin. Nous pouvons créer notre propre nouvelle "balise" `<School />`. Notre balise devrait afficher l'article sur l'école. On passera des props (`name` et `link`) en tant que ses "attributs", et la `description` en tant que son contenu.

Voici ce que nous voudrons pouvoir écrire :

```javascript
const element = (
  <section className="container">
    <h1 className="my-3 text-center">Ma liste des écoles à recommander</h1>
    <School name="Alyra" link="https://alyra.fr">
      Une école au coeur de la blockchain. Fondée par des passionnés et ouverte
      à toutes et tous.
    </School>
    <School name="Simplon" link="https://simplon.co">
      Un réseau de Fabriques solidaires et inclusives qui proposent des
      formations gratuites aux métiers techniques du numérique.
    </School>
  </section>
)
```

### Comment alors créer un component ?

Nous l'avons déjà fait, à un caractère près. Il suffit de changer le "s" dans notre fonction `school` pour un "S" majuscule. Nous venons de créer notre premier component.

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

const element = (
  <section className="container">
    <h1 className="my-3 text-center">Ma liste des écoles à recommander</h1>
    <School
      name="Alyra"
      link="https://alyra.fr"
      description="Une école au coeur de la blockchain"
    />
    <School
      name="Simplon"
      link="https://simplon.co"
      description="Un réseau de Fabriques solidaires et inclusives..."
    />
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
      <p>
        Une école au coeur de <b>la blockchain</b>...
      </p>
    </School>
    <School name="Simplon" link="https://simplon.co">
      <p>Un réseau de Fabriques solidaires et inclusives...</p>
    </School>
  </section>
)

ReactDOM.render(element, document.getElementById("root"))
```

Avec cette approche nous pouvons inclure dans notre description des balises HTML ou imbriquer d'autres components React.

https://codepen.io/alyra/pen/GRZNRqE

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

const element = (
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

ReactDOM.render(element, document.getElementById("root"))
```

https://codepen.io/alyra/pen/poyNoeG

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

## return

Component doit retourner quelque chose, par exemple élément React, un number, un string, un boolean ou `null`, un array ou un React.Fragment.

React component ne peut pas retourner `undefined`, ni un objet `{...}`

```javascript
const MyComponent = () => {}
// Uncaught Error:  Nothing was returned from render
```

```javascript
const MyComponent = () => {
  return null
}
// ceci est valide
```

## Class components

On peut aussi créer des components en utilisant la syntaxe des classes. Nous n'allons pas utiliser cette approche mais il est important de la connaître pour savoir lire le code existant.

```javascript
class School extends React.Component {
  render() {
    return (
      <article className="p-3 mb-3 border shadow">
        <h2 className="text-center">{this.props.name}</h2>
        {this.props.children}
        <SchoolLink name={this.props.name} link={this.props.link} />
      </article>
    )
  }
}

class SchoolLink extends React.Component {
  render() {
    return (
      !!this.props.link && (
        <a
          href={this.props.link}
          className="btn btn-primary btn-sm"
          target="_blank"
          rel="noopener noreferrer"
        >
          En savoir plus sur {this.props.name}
        </a>
      )
    )
  }
}
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
- [Components Dashboard - class](https://codepen.io/alyra/pen/PoNOqgR) | [solution](https://codepen.io/alyra/pen/f4e0e4e46ae2f7dcd17885218fe67e7d)
- [Components Notifications - class](https://codepen.io/alyra/pen/vYGWNbV) | [solution](https://codepen.io/alyra/pen/cd6bd1f539c3a00b1e94a29212e71cdd)
- [Components Button - class](https://codepen.io/alyra/pen/zYqPvVE) | [solution](https://codepen.io/alyra/pen/e6ab2d03049f36972aafdc6cd25dd807)
