# GRID 

Le *CSS Grid Layout* permet de créer des mises en page en divisant l'espace d'affichage en grille (dans deux dimensions).

![Goofonts-grid](https://wptemplates.pehaa.com/assets/alyra/goofonts-grid.png)

[![CSS Grid exemple: Newspaper Layout par Olivia Ng](https://wptemplates.pehaa.com/assets/alyra/grid.jpg)](https://codepen.io/oliviale/full/BaoXOOP)



```html
<div class="container">
  <div class="item item-1">1</div>
  <div class="item item-2">2</div>
  <div class="item item-3">3</div>
  <div class="item item-4">4</div>
  <div class="item item-5">5</div>
  <div class="item item-6">6</div>
  <div class="item item-7">7</div>
</div>
```

Pour mettre en place une grille, nous changeons la propriété `display` pour `grid` de l'élément parent et nous définissons la distribution des colonnes (et parfois aussi des lignes) avec `grid-template-columns` (`grid-template-rows`).

```css
.container {
  display: grid;
  grid-template-columns: 40px 50px auto 50px 40px;
  grid-template-rows: 25% 100px auto;
}
```

Le code ci-dessus génère une grille comme sur l'image suivante. Nous pouvons maintenant placer les éléments enfant du `.container` en nous servant des repères numérotés comme dans l'image :

![](https://wptemplates.pehaa.com/assets/alyra/grid-counting.png)

```css
.container {
  height: 90vh;
  max-width: 900px;
  margin: auto;
  display: grid;
  grid-template-columns: 40px 50px auto 50px 40px;
  grid-template-rows: 25% 100px auto;
}
.item-1 {
  background: pink;
  grid-column: 1/3;
  grid-row: 1/-2;
}
.item-2 {
  background: lavender;
}

.item-3 {
  background: tomato;
}

.item-4 {
  background: teal;
}

.item-5 {
  background: beige;
  grid-column: 3/4;
  grid-row: 2/-1;
}

.item-6 {
  background: cornflowerblue;
  grid-column: span 2;
  grid-row: span 2;
}

.item-7 {
  background: yellowgreen;
  grid-column: 1/3;
}
```

https://codepen.io/alyra/pen/OJRVPZm

https://codepen.io/rachelandrew/pen/BNXyQa

Comme vous pouvez le voir, nous avons beaucoup de flexibilité en distribuant les éléments au sein de la grille. En particulier, en utilisant *grid*, nous pouvons jouer avec l'ordre des éléments :

https://codepen.io/rachelandrew/pen/WvVbMG

Le [playground - grid-template-columns & gap](https://cdpn.io/alyra/debug/96bd1934be6a0cce78c6b32a63fb7c1f) permet de jouer avec ces deux propriétés :

### `grid-template-columns`

- `<length>` - la longueur en `px`, `rems`, etc. 
- `<percentage> - en `%`
- unité `fr` (unité du type `<flex>`) - longueur flexible à l'intérieur d'un conteneur en grille
- `minmax(min, max)` - définit un intervalle de taille entre `min` et `max`
- `repeat` - permet la répéter la même valeur 
  - `repeat(5, 10rem)` est équivalent à `10rem 10rem 10rem 10rem 10rem`
  - `repeat(3, 1fr)` est équivalent à `1fr 1fr 1fr`
  - en utilisant `repeat` avec `auto-fit` ou `auto-fill`, c'est le navigateur qui calcule le nombre de répétitions par ligne optimale est sans déborder de la grille.
  
### `gap` 
  
La propriété `gap` est un raccourci pour `row-gap` et `column-gap` et définit les espaces entre les lignes et entre les colonnes d'une grille.

```css
gap: 10px 20px;
/*
row-gap: 10px;
column-gap: 20px;
*/
```

### Responsive grid sans media queries

Voici la plus simple façon de mettre en place une grille responsive **sans utiliser des media queries** :

 ```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(12rem, 1fr));
  gap: 1rem;
}
```
 
https://codepen.io/alyra/pen/VwKLXgq
 
https://wptemplates.pehaa.com/assets/alyra/grid-repeat.mp4
 

### `grid-template-rows`

`grid-template-rows` fonctionne de la façon similaire que `grid-template-columns`, mais nous l'utilisons beaucoup moins souvent. Dans la plupart des cas, la valeur `auto` (ou `none`) pour la hauteur des lignes est ce qui nous conviendrait le mieux. Dans ce cas-là, nous n'avons pas besoin de définir explicitement la valeur pour `grid-template-rows`. 

[Playground grid-template-rows](https://cdpn.io/alyra/debug/24cb7873e22d7955bd1ad07b1f8aca94)


## Exercices

- [Grid Garden online quiz par Codepip](https://cssgridgarden.com/#fr)
- [CSS Grid ex. 1](https://codepen.io/alyra/pen/GRoqqQW) | [solution](https://codepen.io/alyra/pen/2f5ffa8a8922370dad2c83a54bd4fba9)
- [CSS Grid ex. 2](https://codepen.io/alyra/pen/GRoqqdm) | [solution](https://codepen.io/alyra/pen/d6aa06c2e2c2828249987c87fee580dc)
- [CSS Grid ex. 3](https://codepen.io/alyra/pen/rNxLLrg) | [solution](https://codepen.io/alyra/pen/5238c3a2b2673a6d2d9d43dfafd74c46)
- [CSS Grid ex. 4](https://codepen.io/alyra/pen/bGEeemv) | [solution](https://codepen.io/alyra/pen/29c62d7f596aa543c7a756693eae7500)
- [CSS Grid ex. 5](https://codepen.io/alyra/pen/BajzzvY) | [solution](https://codepen.io/alyra/pen/451d67d8c626285c6d71429b45ae2519)

