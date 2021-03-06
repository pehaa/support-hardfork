# Flexbox

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

### <code>display</code>

```css
.container {
  display: flex;
}
```

---

### <code>flex-direction</code>

Ici on établit l'axe principal d'un container flex sur lequel les items sont disposés. La valeur par défaut est `row`.

```css
.container {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

---

### <code>flex-wrap</code>

Cette propriété définit si le container comprend une seule ligne ou plusieurs. La valeur par défaut est `nowrap`.

```css
.container {
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

![flex-wrap](https://wptemplates.pehaa.com/assets/alyra/flex-wrap.png)

---

### <code>flex-flow</code>

Cette propriété est un raccourci des propriétés `flex-direction` et `flex-wrap`. La valeur par défaut est `row nowrap`.

```css
/* exemple */
.container {
  flex-flow: column wrap;
}
```

- [Playground `flex-flow` (`flex-direction` `flex-wrap`)](https://cdpn.io/alyra/debug/d0ef4e9fed51c6dcf0de9d423b642243)

---

### <code>justify-content</code> 👈

Définit l'alignement le long de l'axe principal en distribuant l'espace qui reste libre. La valeur par défaut est `flex-start`.

```css
.container {
  justify-content: flex-start | flex-end | center | space-between | space-around
    | space-evenly;
}
```

![justify-content](https://wptemplates.pehaa.com/assets/alyra/justify-content.png)

- [Playground `justify-content`](https://cdpn.io/alyra/debug/572a2fe40e375d00d458afdc37768a68)

---

### <code>align-items</code> 👈

Définit la façon dont les items d'une ligne sont disposés le long de l'axe "secondaire". La valeur par défaut est `stretch`.

```css
.container {
  align-items: stretch | flex-start | flex-end | center | baseline;
}
}
```

![align-items](https://wptemplates.pehaa.com/assets/alyra/align-items.png)

- [Playground `align-items`](https://cdpn.io/alyra/debug/9aaec9c3fa5928e7cbf845ccac43ba34)

---

### <code>align-content</code>

Aligne les lignes d'un container flex (si plus qu'une ligne). La valeur par défaut est `stretch`.

```css
.container {
  align-content: flex-start | flex-end | center | space-between | space-around |
    space-evenly | stretch;
}
```

![align-content](https://wptemplates.pehaa.com/assets/alyra/align-content.png)

- [Playground `align-content`](https://cdpn.io/alyra/debug/f32a3e24e05d59a1c4f5f31b7182c9c5)

---

### <code>gap</code> 👈

Raccourci pour `row-gap column-gap` permet de définir les espaces entre les éléments.

```css
.container {
  gap: 1rem;
  /*
  row-gap: 1rem;
  column-gap: 1rem;
  */
}
```

- [Playground `align-content`](https://cdpn.io/alyra/debug/a4466cf0ef4247de582cbe8e9ef65318)

---

## Step 2 : Les "enfants"

### `order`

Par défaut, les items flex sont disposés par ordre d'arrivée, `order` permet de changer l'ordre dans lequel ils apparaissent dans leur parent.  
La valeur par défaut de tous les éléments est 0.

```css
.item-2 {
  order: 1;
}
```

---

### `flex`

Un raccourci de 3 propriétés à la fois `flex-grow`, `flex-shrink` et `flex-basis`.

Chaque élément enfant peut être étiré (c'est `flex-grow` qui est responsable à ça) ou réduit (et c'est `flex-shrink` qui est responsable à ça). La 3e propriété `flex-basis` correspond à la taille de base d'un élément.

La valeur par défaut est `0 1 auto`

[Playground `flex`](https://cdpn.io/alyra/debug/d8c627f95d09e10c1e6f32269bba9493)

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

---

### `align-self`

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

https://codepen.io/alyra/pen/pogyOLJ

---

### `margin: auto`

`margin: auto;` fait de la magie quand appliqué sur un élément enfant.

Valeur `auto` est interprété de la façon : _j'ajoute autant de marge que possible_, regardons :

https://codepen.io/alyra/pen/NWxNLmP

[Playground "big title"](https://cdpn.io/alyra/debug/4b5981bfbfd5ae69b759661cc5de9119)

https://codepen.io/alyra/pen/jOWqepw

---

## Ressources

- [A Complete Guide to Flexbox (en) CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [MDN - Tuto Flexbox - lecture obligatoire](https://developer.mozilla.org/fr/docs/Apprendre/CSS/CSS_layout/Flexbox)
- [Quiz on-line Flexbox Froggy](https://flexboxfroggy.com/#fr)

---

## Exercices

- [Flexbox ex. 1 ](https://codepen.io/alyra/pen/GRoqJxR)
- [Flexbox ex. 1a](https://codepen.io/alyra/pen/PoZzqaP)

---

- [Flexbox ex. 3](https://codepen.io/alyra/pen/VwejvZw)
- [Flexbox ex. 3a](https://codepen.io/alyra/pen/xxZOwxB)
- [Flexbox ex. 4](https://codepen.io/alyra/pen/RwrRWGa)
- [Poulet Tikka](https://codepen.io/alyra/pen/jOVedgN)

---

- [Flexbox ex. 2](https://codepen.io/alyra/pen/WNrxvqO)
- [Flexbox ex. 5](https://codepen.io/alyra/pen/qBbNOod)
- [Design Quotes (flexbox)](https://codepen.io/alyra/pen/XWXraxW)

---

- [Triple Dare - CSS Messy Challenge](https://github.com/pehaa/css-off-hardfork)
