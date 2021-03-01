# Couleurs 🌈

CSS nous permet de colorier nos pages, en particulier avec des propriétés :

- `background-color`
- `color`
- `border`
- `box-shadow`
- `text-shadow`

Il y a plusieurs possibilités pour décrire une couleur. Voici quelques exemples pour de décrire la même nuance du rouge:

- `red`
- `#ff0000`
- `#f00`
- `rgb(255, 0, 0)`
- `rgba(255, 0, 0, 0)`
- `rgb(100%, 0, 0)`
- `hsl(0, 100%, 50%)`

Nous allons maintenant déchiffrer tout cela.

## Couleurs par mot-clé

La première possibilité est d'utiliser le nom de la couleur [keyword](https://developer.mozilla.org/fr/docs/Web/CSS/Type_color#Les_mots-cl%C3%A9s) (mot-clé) : `red`, `black`, `tomato`, `cornflowerblue`, ...., `transparent` et `currentColor`.

De cette façon nous pouvons décrire **147** couleurs différentes, sans compter `transparent` et `currentColor`. Voici un autre [site que les présente](http://www.colors.commutercreative.com/)).

De cette façon, nous sommes capables de référencer une minime partie des couleurs qui peuvent être affichées sur l'écran. Et si nous voulons aller au-delà ?

## Anatomie de la couleur

Chaque pixel sur l'écran est composé de trois petits points appelés luminophores entourés d'un masque noir. Les trois luminophores distincts produisent respectivement de la lumière <span style="color:red;">rouge</span>, <span style="color:lime">verte</span> et <span style="color:blue;">bleue.</span>

Pour contrôler la couleur de chaque pixel sur l'écran, le système d'exploitation consacre une quantité de mémoire à chaque pixel.

Sur les premiers écrans en noir et blanc, pixel est allumé (1) ou éteint (0), un seul morceau de mémoire est affecté à chaque pixel - un bit de mémoire.

Sur les écrans modernes, 8 bits de mémoire sont affectés pour chaque couleur (rouge, vert et bleu). Cela donne 2<sup>8</sup> = **256** valeurs possibles pour la couleur rouge, **256** pour la couleur verte et **256** pour la bleue, donc:

256 x 256 x 256 = **16 777 216** couleurs.

## Format `rgb` et `rgba`

**rgb, rgba** décrit des intensités du rouge (r), vert (g) et b (bleu) + opacité a (alpha)

[Playground - Couleurs RGB expliquée](https://cdpn.io/alyra/debug/b2c543699a8868342fb23ac6c9f6f73d)

```css
p {
  color: rgb(255, 0, 0);
  background: rgb(255, 0, 0, 0.1);
}
```

## Format hexadécimal (#)

Le format **hexadécimal** fonctionne comme le format `rgb`, il regroupe des intensités du rouge, vert et bleu, mais représenté en système hexadécimal.

Vous pouvez en lire davantage dans [cet article sur smashingmagazine.](https://www.smashingmagazine.com/2012/10/the-code-side-of-color/)

[![""](https://wptemplates.pehaa.com/assets/alyra/rgbtohex.png)](https://cdpn.io/alyra/debug/b2c543699a8868342fb23ac6c9f6f73d)

[![Le fameux Quiz du Professeur Hervé B.*](https://wptemplates.pehaa.com/assets/alyra/quizz-rvb.png)](https://cdpn.io/alyra/debug/616e97467780239fc8927073fe284ec5)

## Format `hsl` et `hsla`

Le format couleur HSL, représente une couleur et non pas ses :

- teinte `h` (hue)
- saturation `s`
- clarté `l` (lightness)

[HSL Color Picker par Marton Borbely](https://codepen.io/HunorMarton/full/dvXVvQ)

Le format `hsl` est particulièrement pratique pour gérer :

- les nuances et ombres (**juste le 3e paramètre change**):

```css
/* Teinte de base */
background-color: hsl(14, 76%, 55%);

/* Plus foncée */
background-color: hsl(14, 76%, 75%);

/* Plus claire */
background-color: hsl(14, 76%, 35%);
```

- couleurs complémentaires (ajouter 180 - demi-tour au 1er paramètre):

```css
/* Teinte de base */
color: hsl(14, 76%, 55%);

/* Couleur complémentaire */
color: hsl(194, 76%, 55%);
```

- couleurs complémentaires adjacentes (+/-120 au 1er paramètre):

```css
/* Teinte de base */
color: hsl(14, 76%, 55%);

/* Couleur complémentaire adj. 1 */
color: hsl(134, 76%, 55%);

/* Couleur complémentaire adj. 1 */
color: hsl(254, 76%, 55%);
```

- couleurs similaires (analogous) (+/-30 au 1er paramètre):

```css
/* Teinte de base */
color: hsl(14, 76%, 55%);

/* Couleur complémentaire adj. 1 */
color: hsl(44, 76%, 55%);

/* Couleur complémentaire adj. 1 */
color: hsl(74, 76%, 55%);
```

https://codepen.io/alyra/pen/RwrPBxB

[HSL en action](https://cdpn.io/alyra/debug/LYpoYPY)

## Nouveau format couleur

Des nouvelles couleurs arrivent - pour l'instant uniquement dans le navigateur Safari [CodePen](https://codepen.io/cssgrid/pen/KKpLBom)
Nous n’allons pas les utiliser, mais vous pouvez [en lire ici](https://webkit.org/blog/10042/wide-gamut-color-in-css-with-display-p3/)

## Contrast checker - Web Accessibility in Mind

Vous savez maintenant comment référencer les couleurs. Par contre, toutes les couleurs ne fonctionnent pas bien ensemble. Le problème principal ici est le contraste entre la couleur du fond (`background-color`) et la couleur du texte (`color`). Le contraste trop bas rend notre contenu illisible.

Il existe des normes qui définissent les valeurs correctes pour le contraste, ainsi que des outils qui calculent et valident le contraste.

[Contrast checker](https://webaim.org/resources/contrastchecker/)

## Propriété `background`

La propriété `background` est un raccourci qui regroupe des propriétés liées à la gestion des arrière-plans d'un élément (couleur, image, origine, taille, répétition, etc.)

```css
div {
  background: red;
  /*
  background-color: red;
*/
}
```

```css
div {
  background: red url("mon-image.png");
  /*
  background-color: red;
  background-image: url("mon-image.png");
*/
}
```

Vous pouvez trouver plus de détails et exemples sur [MDN](https://developer.mozilla.org/fr/docs/Web/CSS/background).

### Playgrounds :

- [background-size/background-repeat/background-position](https://codepen.io/alyra/debug/ExPxpyw)
- [background-size/background-repeat/background-position (1)](https://cdpn.io/alyra/debug/ExNLYLd)
- [linear-gradient](https://codepen.io/alyra/debug/bGEdmMM)

## Exercices

- [hsl colors](https://codepen.io/alyra/pen/JjGdBwM)
