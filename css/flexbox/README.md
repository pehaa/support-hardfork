# Flexbox

Le temps est venu d'expérimenter les [layouts plus élaborés...](https://wp-pehaa.xyz/assets/layout.mp4)

Le **Module Flexbox** permet de contrôler la disposition des éléments verticalement ou horizontalement. En parallèle le navigateur essaie d'optimiser l'espace disponible. Grâce au flexbox, on sort facilement du modèle [classique](https://codepen.io/alyra/debug/eYJZMVa) (le contenu qui s'affiche du haut en bas)</a> vers [un layout plus complexe.](https://codepen.io/alyra/debug/e115e4a915987967ec21f41b96c37456?editors=1100)

[Faisons ça ensemble](https://codepen.io/alyra/pen/eYJZMVa)

```html
<div class="container">
  <div class="item item-1">...</div>
  <div class="item item-2">...</div>
  ...
  <div class="item item-9">...</div>
</div>
```

## Step 1 : Élément "parent"

```css
.container {
  display: flex;
}
```

[![](https://wptemplates.pehaa.com/assets/alyra/cheatsheet-flex-parent.png)](https://assets.codepen.io/4515922/FlexCheatsheetParent.pdf)

[Cheatsheet en format .pdf](https://assets.codepen.io/4515922/FlexCheatsheetParent.pdf)

### Playgrounds (testez les propriétés concernant l'élément "parent")

- [`flex-flow` (`flex-direction` `flex-wrap`)](https://cdpn.io/alyra/debug/d0ef4e9fed51c6dcf0de9d423b642243)
- [`justify-content`](https://cdpn.io/alyra/debug/572a2fe40e375d00d458afdc37768a68)
- [`align-items`](https://cdpn.io/alyra/debug/9aaec9c3fa5928e7cbf845ccac43ba34)
---
- [`align-content`](https://cdpn.io/alyra/debug/f32a3e24e05d59a1c4f5f31b7182c9c5)

## Step 2 : Les "enfants"

### propriété `order`

Par défaut, les items flex sont disposés par ordre d'arrivée, `order` permet de changer l'ordre dans lequel ils apparaissent dans leur parent.  
La valeur par défaut de tous les éléments est 0.

```css
.item-2 {
  order: 1;
}
```

### propriété `flex`

Cette propriété est un raccourci de 3 propriétés à la fois `flex-grow`, `flex-shrink` et `flex-basis`.

Chaque élément enfant peut être étiré (c'est `flex-grow` qui est responsable à ça) ou réduit (et c'est `flex-shrink` qui est responsable à ça). La 3e propriété `flex-basis` correspond à la taille de base d'un élément.

La valeur par défaut est `0 1 auto`

**Exemples :**

Nous partons tous sur la même bases et nous prenons chacun la partie égale de l'espace restante.

```css
.item {
  flex: 1;
  /* est équivalent à : */
  /*
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 0%;
  */
}
```

Au départ chacun de nous prend l'espace proportionnel à son contenu. Nous partagerons ensuite l'espace restant également entre nous:

```css
.item {
  flex: auto;
  /* est équivalent à : */
  /*
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: auto;
  */
}
```

[Exemple](https://cdpn.io/alyra/debug/d8c627f95d09e10c1e6f32269bba9493)

Et si ensuite j'ajoute

```css
.item-2 {
  flex: 2;
  /* est équivalent à : */
  /*
  flex-grow: 2;
  flex-shrink: 1;
  flex-basis: 0%;
  */
}
```

`.item-2` aura le droit à deux fois plus de l'espace restant.

https://codepen.io/alyra/pen/GRoZXEX

### propriété `align-self`

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

https://codepen.io/alyra/pen/pogyOLJ

### `margin: auto`

`margin: auto;` fait de la magie quand appliqué sur un élément enfant.

Valeur `auto` est interprété de la façon : _j'ajoute autant de marge que possible_, regardons :

https://codepen.io/alyra/pen/NWxNLmP

[Playground "big title"](https://cdpn.io/alyra/debug/4b5981bfbfd5ae69b759661cc5de9119)

https://codepen.io/alyra/pen/jOWqepw

---

[Quiz on-line Flexbox Froggy](https://flexboxfroggy.com/#fr)

## Ressources

- [A Complete Guide to Flexbox (en) CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [MDN - Tuto Flexbox - lecture obligatoire](https://developer.mozilla.org/fr/docs/Apprendre/CSS/CSS_layout/Flexbox)

---

## Exercices

- [Flexbox ex. 1 ](https://codepen.io/alyra/pen/GRoqJxR) | [solution](https://codepen.io/alyra/pen/84a9f8a69b6243deb2b95fe93aa74316)
- [Flexbox ex. 1a](https://codepen.io/alyra/pen/PoZzqaP) | [solution](https://codepen.io/alyra/pen/8b774f4e71b7eb7946a15dbb378d862b)
- [Flexbox ex. 2](https://codepen.io/alyra/pen/WNrxvqO) | [solution](https://codepen.io/alyra/pen/9a050e3c91e9029cd2096770f37876e4)
- [Flexbox ex. 3](https://codepen.io/alyra/pen/VwejvZw) | [solution](https://codepen.io/alyra/pen/1a040d1d180f068547f1b0c6f931e071)
- [Flexbox ex. 3a](https://codepen.io/alyra/pen/xxZOwxB) | [solution](https://codepen.io/alyra/pen/d9f7f2e28a301e7e3a2b9c70d5d945a6)
- [Flexbox ex. 4](https://codepen.io/alyra/pen/RwrRWGa) | [solution](https://codepen.io/alyra/pen/d88f38824dc4c48923c7f067642baa93)
- [Flexbox ex. 5](https://codepen.io/alyra/pen/qBbNOod) | [solution](https://codepen.io/alyra/pen/4713d0afd1668d23a263bc14ad9b89da)
- [Poulet Tikka](https://codepen.io/alyra/pen/GRpVexo) | [solution](https://codepen.io/alyra/pen/e345eab877855ce5b5c1457688a56fea)
- [Design Quotes (flexbox)](https://codepen.io/alyra/pen/XWXraxW) | [solution](https://codepen.io/alyra/pen/9ede43cefbc1b8e2815f178ffac4507d)

