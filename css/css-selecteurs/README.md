# Sélecteurs CSS

Bien sélectionner les éléments pour changer leurs propriétés CSS est une partie importante de notre apprentissage.

## Différents types de sélecteurs et comment les combiner ?

Pour commencer, ce matin, on vous offre un petit déjeuner, [commandez tout ce qui vous plaît.](https://css-cantine.netlify.app/)

[![css cantine](https://wptemplates.pehaa.com/assets/alyra/css-cantine.png)](https://css-cantine.netlify.app/)

Nous allons aussi utiliser le pen ci-dessous pour jouer avec des sélecteurs :

https://codepen.io/alyra/pen/jOVxbLp

ainsi que [ce site qui traduit un sélecteur entré par l'utilisateur en anglais.](https://hugogiraudel.github.io/selectors-explained/)

## Type Selector

`tag {}`

En français _sélecteur de type._ Les _sélecteurs de type_ ciblent des éléments en fonction du nom de leur balise (`tag`) HTML ou XML, comme `div`, `p`, `ul`, `assiette` etc. C'est un sélecteur simple.

```css
img {
  max-width: 100%;
  height: auto;
}
```

```css
body {
  background: lavender;
}
```

---

## Class Selector

`.class {}`

En français _sélecteur de classe._ Le symbole `.` suivi (sans espace) par le nom de la classe permet de cibler tous les éléments qui ont cette classe. C'est un sélecteur simple.

```css
.mini {
  font-size: 0.875rem;
}
```

---

## ID Selector

`#jolie`

En français _sélecteur d'identifiant._ Avec le symbole `#` suivi (sans espace) par le nom d'identifiant on peut cibler un élément par son attribut `id.` C'est un sélecteur simple.

```css
#top {
  position: fixed;
  top: 0;
}
```

---

## The Universal Selector

`* {}`

En français - sélecteur universel. Il correspond à un élément de n'importe quel type. C'est un sélecteur simple.

```css
* {
  box-sizing: border-box;
}
```

---

## Attribute selector

`[attr] {}`,  
`[attr=value] {}`,  
`[attr*=value] {}`,  
`[attr^=value] {}`,  
`[attr$=value] {}`,  
`[attr~=value] {}`

En français _sélecteur d'attribut._ C'est un sélecteur simple.

<dl>
 <dt><code class="language-text">[<em>attr</em>]</code></dt>
 <dd>Cible des éléments avec l'attribut <code class="language-text">attr</code>.</dd>
 <dt><code class="language-text">[<em>attr</em>=<em>value</em>]</code></dt>
 <dd>Cible  des éléments avec l'attribut <code class="language-text">attr</code> dont la valeur est exactement <code class="language-text">value</code>.</dd>
 <dt><code class="language-text">[<em>attr</em>~=<em>value</em>]</code></dt>
 <dd>Cible des éléments avec l'attribut <code class="language-text">attr</code> dont la valeur contient <code class="language-text">value</code> séparée par des espaces.
 <dt><code class="language-text">[<em>attr</em>|=<em>value</em>]</code></dt>
 <dd>Cible des éléments avec l'attribut <code class="language-text">attr</code> dont la valeur est exactement <code class="language-text">value</code> ou dont la valeur commence par <code class="language-text">value</code> suivi immédiatement d'un tiret (U+002D). Souvent utilisé avec des codes de langues.</dd>
 <dt><code class="language-text">[<em>attr</em>^=<em>value</em>]</code></dt>
 <dd>Cible des éléments avec l'attribut <code class="language-text">attr</code> dont la valeur commence par <code class="language-text">value</code>.</dd>
 <dt><code class="language-text">[<em>attr</em>$=<em>value</em>]</code></dt>
 <dd>Cible des éléments avec l'attribut <code class="language-text">attr</code> dont la valeur se termine par <code class="language-text">value</code>.</dd>
 <dt><code class="language-text">[<em>attr</em>*=<em>value</em>]</code></dt>
 <dd>Cible des éléments avec l'attribut <code class="language-text">attr</code> et dont la valeur contient au moins une occurrence de&nbsp;<code class="language-text">value</code>.</dd>
</dl>

```css
[lang="en"] {
  font-family: italic;
}

a[href^="#"] {
  background-color: gold;
}

a[href$="alyra.fr"] {
  background-color: blue;
  color: white;
}
```

Il existe aussi le sélecteur _lang pseudo-class_ `:lang()`

```css
:lang(en) {
  font-family: italic;
}
```

---

## Descendant Combinator

`A B {}`

En français : le combinateur de descendance. Permet de combiner deux sélecteurs sous la forme `A B`.
`A B` cible des éléments qui correspondent au sélecteur `B` uniquement si ceux-ci ont un élément ancêtre qui correspond au premier sélecteur (`A`).
Attention à l'espace entre deux éléments.

```css
header p {
  font-weight: bold;
}
```

---

## Selector list (,)

`A, B {}`

Cibler en même temps les éléments `A` et `B`. On peut ainsi combiner plusieurs types de sélecteurs et en avoir plus de deux.

```css
h1,
h2,
h3,
.heading {
  font-family: Montserrat, sans-serif;
  font-weight: 900;
}
```

---

Dans les derniers cas ci-dessus, nous utilisons un espacement ou la virgule pour séparer les sélecteurs. Nous arrivons ainsi à combiner des sélecteurs simples.

Mais que se passe-t-il si deux sélecteurs simples sont collés ensemble ? Par exemple `p.mini` ou `#top.mini` ?

En collant des sélecteurs ensemble (sans espace) nous ajoutons des restrictions, `AB` cible des éléments qui correspondent en même temps au sélecteur `A` et `B`.

---

## Child Selector

`A > B {}`

Permet de combiner des sélecteurs. Cible les éléments `B` qui sont des enfants directs des éléments `A`.

```css
header > div {
  border: 3px solid;
}
```

---

## Adjacent Sibling Selector

`A + B {}`

Permet de combiner des sélecteurs. `A + B` cible tous les éléments `B` qui suivent directement les `A`.

```css
h2 + p {
  font-size: 0.875rem;
  text-transform: uppercase;
}
```

---

## General sibling combinator"

`A ~ B {}`

Permet de combiner des sélecteurs. A ~ B cible tous les éléments `B` qui suivent les `A`.

---

## First Child Pseudo-class

`:first-child {}`

Cible les éléments qui sont les premiers enfants de son élément parent.

```css
li:first-child {
  font-weight: bold;
}
```

---

## nth Child Pseudo-class

`:nth-child(..) {}`

Cible les éléments qui sont les enfants numéro .. de son élément parent.

```css
li:nth-child(2n + 1) {
  background: pink;
}
/*
li:nth-child(1) {
  background: pink;
}
li:nth-child(3) {
  background: pink;
}
li:nth-child(5) {
  background: pink;
}
...
*/
```

---

## Last Child Pseudo-class

`:last-child {}`

Cible les éléments qui sont les derniers enfants de son élément parent.

```css
section p:last-child {
  margin-bottom: 0;
}
```

## First-of-type Pseudo-class

`:first-of-type {}`

Cible les éléments qui sont les premiers enfants de leurs types

```css
section p:first-of-type {
  font-size: 1.25rem;
}
```

---

Il existe aussi `only-child`, `nth-last-child`, `last-of-type`, `nth-of-type`, `nth-last-of-type`, `only-of-type`

---

## :not() Pseudo-class

`:not(A) {}`

Utilise la négation pour cibler les éléments. Cible tous les éléments qui ne correspondent pas au sélecteur `A`.

```css
img:not([alt]) {
  border: 5px solid red;
}
```

---

## Autres sélecteurs

- `:root { }` - cible la racine du document (`html`)
- `:hover { }` - l'apparence d'un élément au survole
- `:focus { }` - l'apparence d'un élément au focus
- `:link { }` - permet de sélectionner les liens à l'intérieur d'éléments. Il sélectionnera tout lien n'ayant pas été visité
- `:visited { }` - permet de modifier l'aspect d'un lien après que l'utilisateur l'a visité
- `:empty { }`
- `:target { }`
- `:enabled { }`
- `:disabled { }`
- `:checked { }`
- `:default { }`
- `:valid { }`
- `:invalid { }`
- `:in-range { }`
- `:out-of-range { }`
- `:required { }`
- `:optional { }`
- `:read-only { }`
- `:read-write { }`
- `:right { }`
- `:left { }`

### Pseudo-éléments

- `:before { }` - crée un pseudo-élément qui sera le premier enfant de l'élément sélectionné. Il est souvent utilisé pour ajouter du contenu cosmétique à un élément, en utilisant la propriété CSS `content`.
- `:after { }` - crée un pseudo-élément qui sera le dernier enfant de l'élément sélectionné. Il est souvent utilisé pour ajouter du contenu cosmétique à un élément, en utilisant la propriété CSS `content`.
- `::selection { }` - cible une portion du document qui a été sélectionnée par l'utilisateur
- `:first-letter { }`
- `:first-line { }`

https://codepen.io/alyra/pen/RwrrpBO

[![Bien  vise ?](https://wptemplates.pehaa.com/assets/alyra/quiz-selectors1.png)](https://cdpn.io/alyra/debug/6f79149941afdce6945473428e1395ac)
[Quiz "Bien visé ?"](https://cdpn.io/alyra/debug/6f79149941afdce6945473428e1395ac)

---

## Résolutions de conflits

Avec tous ces types de sélecteurs et la possibilité de les combiner, il est clair que les éléments peuvent être ciblés à plusieurs manières.

Les 2 règles principales sont :

1. Le CSS est lu de haute en bas, les déclarations qui suivent prennent le dessus sur les déclarations précédentes.

1. Nos déclarations prennent toujours le dessur sur le _user agent stylesheet_

Regardons ensemble cet exemple :

```html
<ul class="personnages">
  <li class="list-item">Stormtrooper</li>
  <li class="list-item">Boba Fett</li>
  <li class="list-item">Dark Vador</li>
  <li class="list-item">Empereur Palpatine</li>
</ul>
```

```css
.personnages li {
  color: red;
}
.list-item {
  color: green;
}
ul > li {
  color: blue;
}
```

À votre avis de quelles couleurs seront nos personnages ?

<dl style="font-size:20px">
  <dt>Puissance "0"</dt>
  <dd>Sélecteurs universels <em>universal selector</em> <code class="language-text">*</code></dd>
  <dt>Puissance "1"</dt>
  <dd>
  <em>type selectors</em> <code class="language-text">html</code>, <code class="language-text">body</code>, <code class="language-text">div</code>,  &hellip;<br>
  <em>pseudo-elements</em> <code class="language-text">:before</code>, <code class="language-text">:after</code>, &hellip;</dd>
  <dt>Puissance "10"</dt>
  <dd>
    classes <code class="language-text">.title</code><br>
    attributs <code class="language-text">[class]</code>, <code class="language-text">[id]</code>, <code class="language-text">[title]</code>, <code class="language-text">[href]</code>, &hellip;<br>
    pseudo-classes <code class="language-text">:hover</code>, <code class="language-text">:first-child</code>, <code class="language-text">:lang()</code>, <code class="language-text">:focus</code>, &hellip;<br>
  </dd>
  <dt>Puissance "100"</dt>
  <dd>ids <code class="language-text">#top</code>, <code class="language-text">#contact</code>,</dd>
  <dt>Puissance "1000"</dt>
  <dd> <code class="language-text">style="font-weight:bold"</code> </dd>
  <dt>Puissance "10000"</dt>
  <dd><code class="language-text">{color: red!important;}</code></dd>
</dl>

[![La grille de spécificité en format .pdf](https://wptemplates.pehaa.com/assets/alyra/grille-specificite.png)](https://assets.codepen.io/4515922/Tableau_de_cartes%402x.pdf)

[![Specificity calculator](https://wptemplates.pehaa.com/assets/alyra/calculator.png)](https://specificity.keegan.st/)

![](https://wptemplates.pehaa.com/assets/alyra/mando.jpg)

> Des surspécificités, tu te méfieras… Plus efficaces les sélecteurs de base sont, pour qui sait les manipuler… Plus séducteur est le côté obscur, plus facile… N’en crois rien.

### Ex. 1

https://codepen.io/alyra/pen/MWKaXjG

### Ex. 2

https://codepen.io/alyra/pen/NWxGzbv

[Quizzzzz :](https://cdpn.io/alyra/debug/d341e5aba9eb51c6b9b0f517b45cf812)
[![](https://wptemplates.pehaa.com/assets/alyra/quiz-selectors2.png)](https://cdpn.io/alyra/debug/d341e5aba9eb51c6b9b0f517b45cf812)

[et l'affrontement final :](https://codepen.io/pehaa/debug/dEpvXN)
[![](https://wptemplates.pehaa.com/assets/alyra/quiz-selectors3.png)](https://codepen.io/pehaa/debug/dEpvXN)

## Exercices

👉 Dans tous ces exercices, NE TOUCHEZ PAS AU HTML et privilegier les unités `rem` 👈

- [Selectors - sac à dos](https://codepen.io/alyra/pen/RwrPqYe)
- [Selectors - Statistiques](https://codepen.io/alyra/pen/JjGdeLy)
- [Selectors - damier](https://codepen.io/alyra/pen/MWKwzZe)
- [Selectors - lorem](https://codepen.io/alyra/pen/gOPpQVJ)
