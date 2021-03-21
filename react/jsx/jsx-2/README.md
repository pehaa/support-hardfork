# Syntaxe JSX en détails (2) - Notions avancées

## Expressions JavaScript

JSX permet de faire un mixte de la structure HTML et des expressions JavaScript. Expressions JavaScript sont intégrées entre des accolades (ceci ressemble un peu aux _template literals_).

---

Pour rappel : **une expression JavaScript** est un un code qui donne une valeur en tant que le résultat. Autrement dit, l'expression js est un bout de code qui pourrait se trouver à droite d'un symbole `=`.

---

```javascript
const element = <p>Vous avez {10 + 1} notifications.</p>
// <p>Vous avez 11 notifications.</p>
```

```javascript
const lang = "en"
const element = (
  <h1 lang={lang}>{lang === "en" ? "Hello World!" : "Bonjour le Monde !"}</h1>
)
// <h1 lang="en">Hello World!</h1>
```

```javascript
const lang = "fr"
const element = (
  <h1 lang={lang}>{lang === "en" ? "Hello World!" : "Bonjour le Monde !"}</h1>
)
// <h1 lang="fr">Bonjour le Monde !</h1>
```

```javascript
function capitalize(string) {
  return string[0].toUpperCase() + string.slice(1).toLowerCase()
}
const name = "paulIna"
const element = <h1>Hello {capitalize(name)}</h1>
// <h1>Hello Paulina</h1>
```

**Attention:** On peut de cette façon intégrer uniquement des **expressions** JavaScript. On ne peut pas écrire n'importe quel code js entre les accolades.

Exemples **valides** ✅ :

```javascript
<div id={myVariable}>
  <h2>{array.length ? "Les résultats :" : "Pas de résultats"}</h2>
  <p>{2 ** 16}</p>
  <p>{myFunction()}</p>
</div>
```

Exemples **non valides** 🚫 :

```javascript
<div id={const myVariable = 'top'}>
  <h2>{if (array.length) {Les résultats :'} else {'Pas de résultats'}}</h2>
</div>
```

**Attention** aux apostrophes, si on ajoute des apostrophes autour des accolades, l'ensemble est traité en tant que `string`. Voici un exemple de l'utilisation erronée :

```javascript
const lang = "en"
const element = <h1 lang="{lang}">Hello World!</h1> // 🚫

// <h1 lang="{lang}">Hello World!</h1>
```

versus l'utilisation correcte :

```javascript
const lang = "en"
const element = <h1 lang={lang}>Hello World!</h1> // ✅

// <h1 lang="en">Hello World!</h1>
```

JSX est aussi une expression JavaScript.

```javascript
const bold = true
const element = <h1>{bold ? <b>texte important</b> : <span>texte</span>}</h1> // ✅

// <h1 lang="en">Hello World!</h1>
```

## Espaces vides

JSX supprime les espaces en début et en fin de ligne. Il supprime également les lignes vides. Les sauts de lignes adjacents aux balises sont retirés.

```javascript
const element = (
  <p>
    Les composants vous permettent de découper l’interface utilisateur en
    éléments indépendants et réutilisables,{" "}
    <strong>
      vous permettant ainsi de considérer chaque élément de manière isolée.
    </strong>
  </p>
)
```

Cet espace vide `{" "}` que vous voyez dans le code ci-dessus, vient d'être ajouté par _Prettier_ (l'extension de l'éditeur qui formate le code) quand j'ai sauvegardé le code suivant :

```javascript
const element = (
  <p>
    Les composants vous permettent de découper l’interface utilisateur en
    éléments indépendants et réutilisables,
    <strong>
      vous permettant ainsi de considérer chaque élément de manière isolée.
    </strong>
  </p>
)
```

Dans ce cas Prettier "force" l'utilisation d'une espace entre "réutilisables," et "`<strong>`".

Vous pouvez voir le rendu [sans `{" "}` ici.](https://codepen.io/alyra/pen/OJNxRJV)

![](https://assets.codepen.io/4515922/spaces.png)

## Expressions booléennes et conditional rendering

Les expressions booléennes, ainsi que `undefined` et `null` ne génèrent pas de rendu :

```javascript
const age = 10
const element = <span>{age >= 10}</span>
// <span></span>
```

```javascript
const age = 10
const element = <span>{undefined}</span>
// <span></span>
```

```javascript
const age = 10
const element = <span>{null}</span>
// <span></span>
```

### Symbole &&

Les expressions booléennes peuvent être utilisées afin d'afficher un rendu selon une condition. En particulier `&&` est souvent utilisé dans JSX.

```javascript
const name = "Alyra"
const element = (
  <header>
    <h1>Bienvenue</h1>
    {!!name && <p>Notre école s'appelle {name}</p>}
  </header>
)
/*
<header>
  <h1>Bienvenue</h1>
  <p>Notre école s'appelle Alyra</p>
</header>
*/
```

```javascript
const alien = {
  age: 100,
  name: "Deej",
}
const element = (
  <section>
    <h2>Bienvenu {alien.name} !</h2>
    {alien.age >= 100 && <button>Partez en mission</button>}
  </section>
)
/* 
<section>
  <h2>Bienvenu Deej !</h1>
  <button>Partez en mission</button>
</section>
*/
```

### Conditions ternaires

Les conditions ternaires sont également souvent utilisées avec JSX, en voici quelques exemples :

```javascript
const lang = "en"
const element = <h1 lang={lang}>{lang === "fr" ? "Bienvenue" : "Welcome"}</h1>
```

```javascript
const shoppingList = ["Miel", "Sel", "Cumin"]
const length = shoppingList.length
const element = (
  <section>
    <h2>Shopping List</h2>
    {length ? (
      <p>Vous avez {length} articles à acheter.</p>
    ) : (
      <p>Avez-vous besoin de quelque chose ?</p>
    )}
  </section>
)
```

## La syntaxe de décompositions aka props spread

Imaginons que nous avons plusieurs propriétés à passer à un élément. Ces propriétés sont regroupées dans un objet `props` comme ceci :

```javascript
const props = {
  lang: "en",
  id: "top",
  className: "display-1",
}
```

Au lieu de passer propriété par propriété :

```javascript
const element = (
  <h1 lang={props.lang} id={props.id} className={props.className}>
    Hello World
  </h1>
)
```

nous pouvons utiliser la syntaxe de décomposition (_spread syntax_) `...`

```javascript
const element = <h1 {...props}>Hello World</h1>
/*
<h1 lang="en" id="top" class="display-1">Hello World</h1>
*/
```

L'utilisation de _spread_ n'exclut pas l'ajout d'autres propriétés. Regardons l'exemple suivant :

```
const props = {
  lang: "en",
  id: "top",
  className: "display-1"
}
const element = <h1 className="display-4" {...props} lang="fr">Bonjour le Monde</h1>
```

Est-ce correct ? Tout à fait ! Quel sera son rendu ?

Afin de répondre à cette question, regardons sous le capot de babel, ou plutôt en quoi ce code est compilé.

```javascript
React.createElement(
"h1",
Object.assign({className: "display-4"}, props, { lang: "fr" }),
Bonjour le Monde
)
```

Nous devons alors répondre à la question suivante : Quel est le résultat de `Object.assign({className: "display-4"}, props, { lang: "fr" })`

```javascript
Object.assign({className: "display-4"}, props, { lang: "fr" })
//
Object.assign({className: "display-4"}, {lang: "en", id: "top", className: "display-1"}, { lang: "fr" })
// -> valeurs à droite l'emportent sur celles a gauche
{className: "display-1", lang: "fr", id: "top"}
```

Le rendu HTML sera alors

```html
<h1 class="display-1" lang="fr" id="top">Bonjour le Monde</h1>
```

## Arrays

Souvent le contenu que nous devons rendre sur la page nous parvient structuré dans un _array_.
JSX permet de rendre un _array_

```javascript
const shoppingList = ["miel", "sucre", "cumin", "curry"]

const element = (
  <ul>
    {shoppingList}
  </ul>
)
*/
```

Le code ci-dessus fonctionnera mais le rendu ne sera pas correct. Nous devons convertir chaque élément de notre `shoppingList` en un élément `li`. Pour ceci, nous allons utiliser la méthode `map` :

```javascript
const shoppingList

const element = (
  <ul>
    {shoppingList.map((el) => (
      <li key={el}>{el}</li>
    ))}
  </ul>
)
```

### Attribut spécial `key`

Chaque élément d'un array devrait avoir un attribut `key` avec des valeurs uniques, une omission de cet attribut provoquera un warning dans la console. Une clé unique pour chaque élément de la liste permet au React d'effectuer correctement et efficacement son algorithme de comparaison (_diffing algorithm_).

https://codepen.io/alyra/pen/MWyvGRZ

Ici nous pouvons comparer 2 listes, une avec des attributs `key` (👍), l'autre sans (👎). Ouvrez DevTools et observez comment chacune de ces 2 listes est re-rendue. Dans le cas 👍, les quatre éléments `<li>...</li>` ne sont pas re-rendus. Dans le cas 👎, à chaque `ReactDOM.render` tous les éléments `<li>...</li>` sont "refaits" à nouveau.

https://wptemplates.pehaa.com/assets/alyra/shopping-list.mp4

### Exemples avec `.map` et `key`

- article, titre et contenu

```javascript
const mySchools = [
  {
    name: "Alyra",
    description:
      "Une école au coeur de la blockchain. Fondée par des passionnés et ouverte à toutes et tous.",
  },
  {
    name: "Simplon",
    description:
      "Un réseau de Fabriques solidaires et inclusives qui proposent des formations gratuites aux métiers techniques du numérique.",
  },
]

const element = (
  <section>
    <h1>Mes écoles à recommander</h1>
    {mySchools.map((school) => {
      return (
        <article key={school.name}>
          <h2>{school.name}</h2>
          <p>{school.description}</p>
        </article>
      )
    })}
  </section>
)
```

- avec `React.Fragment`

```javascript
const definitions = [
  { term: "React", description: "une librarie JavaScript" },
  { term: "JSX", description: "une extension syntaxique de JavaScript" },
  { term: "Gatsby", description: "un framework basé sur React" },
]

const element = (
  <dl>
    {definitions.map((el) => {
      return (
        <React.Fragment key={el.term}>
          <dt>{el.term}</dt>
          <dd>{el.description}</dd>
        </React.Fragment>
      )
    })}
  </dl>
)
```

Dans ce cas, on est obligé d'utiliser `<React.Fragment>` plutôt que `<>`. Il est impossible d'attribuer `key` à `<>`.

---

## Exercices

- [JSX expression - hello](https://codepen.io/alyra/pen/qBZXEje) | [solution](https://codepen.io/alyra/pen/3e7e235fbb4c229a14585e3b8ef867df)
- [JSX - expressions - logged-in](https://codepen.io/alyra/pen/RwaZNaW) | [solution](https://codepen.io/alyra/pen/43004448ad3a73d223d7f1c1eb6598e7)
- [JSX - expressions - notifications](https://codepen.io/alyra/pen/poyrvEa) | [solution](https://codepen.io/alyra/pen/bc89eb35101c603941bdae0a9b881dfb)
- [JSX - expressions - notifications 1](https://codepen.io/alyra/pen/JjXyxjo) | [solution](https://codepen.io/alyra/pen/bdf636434aa83dde7e1e159b4e78dccc)
- [JSX - notifications list](https://codepen.io/alyra/pen/YzqxMEP) | [solution](https://codepen.io/alyra/pen/f7801accec0de64a8473089fd051f25d)
- [JSX - notifications title + list](https://codepen.io/alyra/pen/bGprJaW) | [solution](https://codepen.io/alyra/pen/10413263fabe4668bf815a7d2ad05ec8)
- [JSX button](https://codepen.io/alyra/pen/YzqxoQO) | [solution](https://codepen.io/alyra/pen/d90868a92c92e2736befa9685f38a1e4)
- [JSX title](https://codepen.io/alyra/pen/VwaMbMP) | [solution](https://codepen.io/alyra/pen/d90868a92c92e2736befa9685f38a1e4)
- [JSX smoothie](https://codepen.io/alyra/pen/xxVXdWZ) | [solution](https://codepen.io/alyra/pen/40a70e8ed7e387d45c8a2581e15efc4d)
