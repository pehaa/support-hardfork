# SASS - premiers pas

Sass est un langage de feuille de style qui est **compilé en CSS.** Sass facilite et automatise l'écriture des styles. Il introduit des éléments des langages de programmation tels que : variables, fonctions, boucles et conditions. Il permet de mieux organiser le code en le divisant en multiples petits fichiers.

Dans le prochain cours, nous allons apprendre comment mettre en place et configurer le compilateur Sass dans Visual Studio Code.

Pour l'instant, nous allons utiliser le compilateur intégré de CodePen. Pour l'activer, choisissez _CSS Preprocessor_ > SCSS dans les réglages du panneau CSS

https://wptemplates.pehaa.com/assets/alyra/scss.mp4

Nous allons utiliser la syntaxe SCSS du Sass. Elle est compatible avec CSS, ce qui veut dire que nous pouvons continuer d'écrire du pure CSS sans avoir des erreurs. Nous pouvons dire que la syntaxe SCSS du Sass est une extension du CSS.

🍒 Vous pouvez aussi utiliser le compilateur en ligne [sassmeister.com](https://www.sassmeister.com/)

## Variables Sass

Avec Sass nous pouvons utiliser des variables. Pour mettre en place une variable Sass, on attribue une valeur à un nom qui commence (obligatoirement) par le symbole `$`. Ensuite on peut faire référence à ce nom au lieu de la valeur elle-même.  
Les variables permettent de réduire les répétitions, car la valeur est mise en place qu'une seule fois.  Ceci est très pratique, et correspond au principe très connu dans la programmation informatique - [DRY (*Don't Repeat Yourself*)](https://fr.wikipedia.org/wiki/Ne_vous_r%C3%A9p%C3%A9tez_pas).

```scss
// scss
$brand-color: tomato;
$base-spacing: 1.5rem;

body {
  border: $base-spacing solid $brand-color;
}

a {
  color: $brand-color;
}

p,
h1,
h2,
h3 {
  margin-bottom: $base-spacing;
}

article {
  border: 1px dotted $brand-color;
  padding: ($base-spacing / 2);
}
```

sera compilé en :

```css
/* css */
body {
  border: 1.5rem solid tomato;
}

a {
  color: tomato;
}

p,
h1,
h2,
h3 {
  margin-bottom: 1.5rem;
}

article {
  border: 1px dotted tomato;
  padding: 0.75rem;
}
```

Pendant la compilation, chaque instance de la variable dans le code sera remplacée par sa valeur. Et, comme vous pouvez l'observer, et ce qui est très pratique, le compilateur effectue aussi des calculs.

## Nesting (Imbrication des sélecteurs)

Sass permet d'imbriquer des sélecteurs, par exemple :

```scss
//  scss
article {
  background: pink;
  padding: 1rem;
  h2 {
    text-align: center;
    a {
      text-decoration: none;
      color: inherit;
    }
  }
  p:last-child {
    border-bottom: 0;
  }
}
```

sera compilé en :

```css
/* css */
article {
  background: pink;
  padding: 1rem;
}
article h2 {
  text-align: center;
}
article h2 a {
  text-decoration: none;
  color: inherit;
}
article p:last-child {
  border-bottom: 0;
}
```

Nous pouvons aussi faire appel au sélecteur courant (`&`) :

```scss
// scss
article {
  h2 {
    a {
      color: red;
      &:hover {
        color: magenta;
      }
    }
  }
}
```

compile vers

```css
/* css */
article h a {
  color: red;
}
article h a:hover {
  color: magenta;
}
```

La référence au sélecteur courant peut être aussi utilisée avec les noms des classes :

```scss
// scss
.button {
  border: 2px solid;
  display: inline-block;
  padding: 1rem;
  &-small {
    padding: 0.5rem;
  }
  &-large {
    padding: 1.5rem;
  }
}
```

compile vers :

```css
/* css */
.button {
  border: 2px solid;
  display: inline-block;
  padding: 1rem;
}
.button-small {
  padding: 0.5rem;
}
.button-large {
  padding: 1.5rem;
}
```

Par contre, la fonctionnalité _nesting_ devrait être utilisée **avec modération,**, car :

- 👎 le code peut devenir moins lisible,
- 👎 les sélecteurs deviennent très longs,
- 👎 la spécificité des sélecteurs peut augmenter considérablement.

## Mixins

*Mixins* (`@mixin`) permettent d'écrire une fois une partie du style et le réutiliser dans plusieurs endroits. Ceci est très pratique, et correspond aussi au principe DRY.

```scss
//scss

// comment définir un mixin
@mixin mixin-name {
  property1: value1;
  property2: value2;
}

// et comment l'utiliser
selector {
  @include mixin-name;
}
```

```scss
// scss
@mixin topleft {
  position: absolute;
  top: 0;
  left: 0;
}

section {
  position: relative;
  .promo {
    @include topleft;
  }
}
```

```css
/* css */
section {
  position: relative;
}
section .promo {
  position: absolute;
  top: 0;
  left: 0;
}
```

## Fonctions

Fonctions Sass permettent de répéter une procédure dans plusieurs endroits.

```scss
// scss
@function function-name($parameter1, $parameter2) {
  @return ....;
}
```

Voici un exemple simple, où nous allons adorer des fonctions Sass. Si vous n'êtes pas à l'aise avec les conversions des pixels vers rems, vous pouvez écrire une fonction qui le fera pour vous :

```scss
// scss
@function px-to-rem($px) {
  @return ($px / 16) * 1rem;
}

h1 {
  font-size: px-to-rem(48);
}

h2 {
  font-size: px-to-rem(42);
}
```

compilera vers

```css
/* css */
h1 {
  font-size: 3rem;
}

h2 {
  font-size: 2.625rem;
}
```

---

# Exercices

- [SASS header + section](https://codepen.io/alyra/pen/bGEqQPE) | [solution](https://codepen.io/alyra/pen/298a4414768901cd75a164605736cbf1)
- [SASS nesting](https://codepen.io/alyra/pen/xxZqmLM) | [solution](https://codepen.io/alyra/pen/7c103eba03c079c5736c8fee15f28670)
- [SASS refactor Marie la Photographe](https://codepen.io/alyra/pen/jOWwLaV) | [solution](https://codepen.io/alyra/pen/f9c226199f0301284222f97fc9d4e76a)
- [SASS BEM](https://codepen.io/alyra/pen/QWypYqJ) | [solution](https://codepen.io/alyra/pen/47c4875be5a8cd92625d24ed9f5dafb6)

