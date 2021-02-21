# Responsive Images <span role="img" aria-label="photo">🌄</span>

## attribut `srcset`

Avez-vous un appareil avec l'écran "haute définition" ? Votre smartphone par exemple ?
Si oui, vous avez peut-être déjà remarqué que parfois, certaines images [(souvent des logos) s'affichent de la façon un peu "floue"...](https://cdpn.io/alyra/debug/vYNPMvw)

Regardons le code, ci-dessous,

```html
<img src="img-300x400.jpg" width="300" alt="texte alternatif" />
```

Sur des écrans haute définition, l'image ci-dessus, va être affichée sur la grille de pixels qui fait 600 points lumineux sur 800 points lumineux (écrans 2x) ou même 900 x 1200 (écrans 3x). C'est comme si on imprimait une image de 10cm sur 10cm sur une feuille 20cm sur 20cm. Ce n'est souvent pas beau du tout...

Regardons comment on peut y remédier ?

### 🤔 Solution 1 : 1x, 2x, (3x)

Première étape est de préparer une autre image, de la taille `600px x 800px` (on double la largeur et la hauteur).
Nous allons utiliser un attribut `srcset` avec des descripteurs `x` (`1x`, `2x`, `3x`) correspondant aux différentes densités de pixels de nos écrans.

```html
<img
  src="img-300x400.jpg"
  srcset="img-300x400.jpg 1x, img-600x800.jpg 2x"
  alt="texte alternatif"
  width="300"
/>
```

Nous pouvons aller encore plus loin. Si nous avons la possibilité d'avoir notre image dans la résolution `900px x 1200px`, utilisons aussi cette variante (img-900-1200.jpg) :

```html
<img
  src="img-300x400.jpg"
  srcset="img-300x400.jpg 1x, img-600x800.jpg 2x, img-900x1200.jpg 3x"
  alt="texte alternatif"
  width="300"
/>
```

Ici on indique au navigateur quelle image est préparée pour quelle résolution.

### 🤔 Solution 2 : unités `w`

Pareil, on prépare une autre image, de la taille `600px x 800px` (on double la largeur et la hauteur) (ainsi qu'une image "triplée" en dimenstions si possible).

```html
<img
  src="img-300x400.jpg"
  srcset="img-300x400.jpg 300w, img-600x800.jpg 600w"
  sizes="300px"
  alt="texte alternatif"
/>
```

Attention: On utilise `w` (width) au lieu de `px`  
Ici on donne au navigateur l'information sur la largeur (originalle des images) et navigateur fait des calcules (en prenant en compte la résolution <strong>et la taille de l'écran</strong>) quelle image choisir.

Il est aussi possible de varier la taille d'affichage d'une image :

```html
<img
  src="img-300x400.jpg"
  srcset="img-300x400.jpg 300w, img-600x800.jpg 600w"
  sizes="(max-width: 600px) 200px, 300px"
  alt="texte alternatif"
/>
```

Ceci veut dire, si la taille de l'écran ne dépasse pas `600px`, l'image prend `200px` de la largeur. Au-dessus de `600px` l'image prendra `300px`.

### 🤔 Solution 3 : format `SVG`

`SVG` (Scalable Vector Graphics) est un format vectoriel. Ceci dit, que cette solution ne s'appliquera pas aux photos. Par contre, elle est <b>parfaite 😍</b> pour les logos, pictos, illustrations. Un SVG ne perdra rien de sa qualité, peu importe la taille. On est souvent gagnants en ce qui concerne le poids d'images (en B) et regardez, on revient au markup tout simple :

```html
<img src="img.svg" width="300" alt="texte alternatif" />
```

## `<picture>` - HTML tag

Il arrive parfois (rarement, mais ça peut arriver) qu'on veuille afficher des images différentes selon la taille de l'écran. Est-ce possible, uniquement avec HTML ? Oui !!!

```html
<picture>
  <source srcset="img1.jpg" media="(min-width: 800px)" />
  <img src="img2.jpg" alt="Le texte alternatif, comme d'hab" />
</picture>
```

On peut aller plus loin :

```html
<picture>
  <source
    media="(max-width:420px)"
    srcset="foxsmall-64x64.png 1x, foxsmall-128x128.png 2x"
    width="64"
  />
  <source
    media="(min-width:421px)"
    srcset="foxfull-140x140.png 1x, foxfull-280x280.png 2x"
    width="140"
  />
  <img src="foxsmall-64x64.png" alt="Origami Fox" />
</picture>
```

**Attention**: Le comportement des navigateurs "on resize" peut varier.

## Exemples

- logo le Parisien

https://codepen.io/alyra/pen/vYNPMvw

- Origami Fox

https://codepen.io/alyra/pen/BaobgZP

## Lecture supplémentaire :

- [RespImageLint Linters](https://ausi.github.io/respimagelint/docs.html)
- [A Guide to the Responsive Images Syntax in HTML](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/)

---

## Exercices

- [Responsive Images](https://codepen.io/alyra/pen/YzyOvgB)
- [Personal Landing Page](https://codepen.io/alyra/pen/WNQgyBw)
