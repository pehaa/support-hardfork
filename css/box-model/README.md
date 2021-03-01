# Box Model 📦

Du point de vue de CSS chaque élément est un box (boîte). Nous allons apprendre comment ces boîtes fonctionnent afin de pouvoir gérer leurs dimensions leur positionnement au sein d'une page HTML.

## Dimensions

Les quatre propriétés `width`, `height`, `padding` et `border` agissent directement sur les dimensions d'un élément.

- `width` - la largeur de la boîte, par défaut, sa valeur est `auto` (en lire plus sur [MDN](https://developer.mozilla.org/fr/docs/Web/CSS/width))
- `height`- la hauteur de la boîte, par défaut, sa valeur est `auto` (en lire plus sur [MDN](https://developer.mozilla.org/fr/docs/Web/CSS/height))

Nous allons aussi parler de la propriété `margin`.

### `padding`

`padding` - raccourcie pour `padding-top` 👆, `padding-right` 👉, `padding-bottom` 👇 et `padding-left` 👈 - les espacements (écarts) internes dans l'élément (en lire plus sur [MDN](https://developer.mozilla.org/fr/docs/Web/CSS/padding))

```css
div {
  /* top, right, bottom et left à 16px */
  padding: 16px;
  /*
  padding-top: 16px;
  padding-right: 16px;
  padding-bottom: 16px;
  padding-left: 16px;
 */
}
```

```css
div {
  /* top et bottom (↕️) à 16px et right et left (↔️) à 24px */
  padding: 16px 24px;
  /*
  padding-top: 16px;
  padding-right: 24px;
  padding-bottom: 16px;
  padding-left: 24px;
*/
}
```

```css
div {
  /* top à 16px et right et left (↔️) à 24px, bottom à 32px  */
  padding: 16px 24px 32px;
  /*
  padding-top: 16px;
  padding-right: 24px;
  padding-bottom: 32px;
  padding-left: 24px;
*/
}
```

```css
div {
  /* top à 16px et right à 24px, bottom à 32px et left à 48px 👆👉👇👈 */
  padding: 16px 24px 32px 48px;
  /*
  padding-top: 16px;
  padding-right: 24px;
  padding-bottom: 32px;
  padding-left: 48px;
*/
}
```

https://codepen.io/alyra/pen/XWXWRXJ

### `border`

- `border` - une propriété raccourcie qui permet de la bordure sa largeur avec `border-width`, son style `border-style` (`solid`, `dashed`, `dotted`, ...) et sa couleur `border-color`.  
  Il existe aussi `border-top` (`border-top-width`, `border-top-style`, `border-top-color`), `border-right`, `border-bottom` et `border-left`.

```css
div {
  border: 2px solid red;
  /*
  border-width: 2px;
  border-style: solid;
  border-color: red;
*/
}
```

```css
div {
  border-bottom: 1px solid;
  /*
  border-bottom-width: 1px;
  border-bottom-style: solid;
*/
}
```

Si la couleur n'est pas spécifiée, la couleur de l'élément est utilisée.

### `margin`

À l'occasion nous allons parler aussi de la propriété `margin` - la taille des marges sur les quatre côtés de l'élément, un raccourci pour `margin-top`, `margin-right`, `margin-bottom` et `margin-left`.

```css
div {
  /* top, right, bottom et left à 16px */
  margin: 16px;
  /*
  margin-top: 16px;
  margin-right: 16px;
  margin-bottom: 16px;
  margin-left: 16px;
 */
}
```

```css
div {
  /* top et bottom (↕️) à 16px et right et left (↔️) à 24px */
  margin: 16px 24px;
  /*
  margin-top: 16px;
  margin-right: 24px;
  margin-bottom: 16px;
  margin-left: 24px;
*/
}
```

```css
div {
  /* top à 16px et right et left (↔️) à 24px, bottom à 32px  */
  margin: 16px 24px 32px;
  /*
  margin-top: 16px;
  margin-right: 24px;
  margin-bottom: 32px;
  margin-left: 24px;
*/
}
```

```css
div {
  /* top à 16px et right à 24px, bottom à 32px et left à 48px 👆👉👇👈 */
  margin: 16px 24px 32px 48px;
  /*
  margin-top: 16px;
  margin-right: 24px;
  margin-bottom: 32px;
  margin-left: 48px;
*/
}
```

Il est important de comprendre la différence entre `margin` et `padding`

[Playground](https://cdpn.io/alyra/debug/NWRKLWy)

![""](https://wptemplates.pehaa.com/assets/alyra/margin-padding.png)

## Élément de type `inline` vs. éléments de type `block`

En CSS, il existe deux type de boîtes : les boîtes de type `block` et les boîtes en ligne (`inline`).

**Un élément de type `block` :**

- veut remplir tout l'espace disponible son conteneur
- crée un retour à la ligne, éléments suivants passent à la ligne
- respecte les propriétés `width` et `height`,
- `padding`, `margin` et `border` repoussent des éléments autour

`p, div, h1-h6, section, ...` la majorité des éléments est type `block`

**Un élément de type `inline` :**

- ne crée pas de retour à la ligne, les autres éléments se suivent en ligne,
- `width` et `height` ne s'appliquent pas
- `padding`, `margin` et `border` sur l'axe vertical sont appliquées, mais ne provoquent pas de déplacement des éléments autour.

`a, span, em, strong, i, b, ins, del ...` - ont le caractère en-ligne `inline`

Le comportement d'un élément peut être modifié avec la propriété `display`

https://codepen.io/alyra/pen/YzwKodK

## Collapsing margins (marges fusionnées)

Les marges verticales (`margin-top` et `margin-bottom`) des éléments type `block` sont parfois fusionnées en une seule marge. C'est ce qu'on appelle _collapsing margins._
La règle qui s'y applique est suivante :

La taille de la marge = la plus grande des deux marges fusionnées.

Ceci concerne les situations suivantes:

- des éléments voisins adjacents
- aucun contenu, bordure ou padding séparant le parent et ses descendants
- bloc vide

Vous pouvez en lire d'avantage sur [MDN.](https://developer.mozilla.org/fr/docs/Web/CSS/Mod%C3%A8le_de_bo%C3%AEte_CSS/Fusion_des_marges)

https://codepen.io/alyra/pen/QWyWprr

## `box-sizing`

Par défault, pour tous les éléments, la valeur de la propriété `box-sizing` est `content-box`. Qu'est-ce que ça implique pour nous ?

Avec `box-sizing: content-box;`, les propriétés `width` et `height` affectées à un élément s'applique à son contenu (son `content-box`). Ni les bordures ni les paddings ne sont pas pris en compte. Ceci n'est pas très pratique pour les développeurs qui doivent ajuster la valeur de `width` et `height`.

[Playground](https://cdpn.io/alyra/debug/416abba364963b2efce1b467ed776f87)

Nous pouvons changer la valeur de `box-sizing` pour `border-box`. Pour l'appliquer pour tous les éléments nous allons utiliser le sélecteur universel (`*`) comme ceci :

```css
* {
  box-sizing: border-box;
}
```

ou comme cela :

```css
html {
  box-sizing: border-box;
}
*,
*:before,
*:after {
  box-sizing: inherit;
}
```
