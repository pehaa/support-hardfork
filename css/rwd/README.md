# Responsive Web Design

Ça fait déjà quelques année que les pages web sont consultées majoritairement sur mobile.

Le terme _Responsive Web Design (RWD)_ date de 2010, il était introduit dans [cet article](https://alistapart.com/article/responsive-web-design/) en 2010 par Ethan Marcotte. RWD décrit une approche du web design qui prend soin que les sites web s'adaptent à leur environnement, en particulier à la taille de l'écran sur lequel ils sont consultés. Par exemple sur un grand écran (desktop d'un Mac ou un PC) nous allons voir un layout composé de plusieurs colonnes, sur un écran mobile le même contenu sera affiché dans une seule colonne.

![](https://wptemplates.pehaa.com/assets/alyra/diseno-web-responsive-design.jpg)

## Mise en place du `viewport`

La première étape de la préparation d'une page "responsive" se déroule dans la partie `head` du document HTML. C'est la ligne `<meta name="viewport" content="width=device-width, initial-scale=1">`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <!-- ... -->
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <!-- ... -->
  </head>
  …
</html>
```

Par défaut les navigateurs mobiles affichent une page web à une largeur d'écran "desktop" (souvent 980px). Par conséquent tous les éléments (et les tailles de police) sont mis à l'échelle. Pour y remédier nous indiquons aux navigateur mobile de respecter la largeur en pixels de l'appareil en affichant la page.

Pour être plus précis nous parlons ici des _density-independent_ (_device-independent_) pixels. Un _density-independent_ pixel ne prend pas en compte la densité des pixels, sur un écran haute densité (HD, Rétina) un _density-independent_ pixel se compose de nombreux pixels physiques.

![](https://wptemplates.pehaa.com/assets/alyra/viewport.jpg)

Nous pouvons aussi comparer cet [exemple sans `meta viewport`](https://without-vp-meta.glitch.me/) et [celui avec `meta viewport`.](https://with-vp-meta.glitch.me/)

## Live coding [Marie la Photographe](https://cdpn.io/alyra/debug/88d72ae8d0d2618620106b56c2bfbc52)

https://codepen.io/alyra/pen/ZEQYYEr

https://codepen.io/alyra/pen/ZEBqbEp

## Images

Chaque image a ses dimensions fixes (physiques) et si une image est plus grande que la fenêtre, elle entraînera l'apparition d'une barre de scroll horizontalle.

Voici comment y remédier :

```css
img {
  /* l'image ne dépassera pas horizontalement */
  max-width: 100%;
  /* la ratio sera préservé */
  height: auto;
}
```

## La largeur du texte

Le contenu textuel ne provoque pas des problèmes comme les images. Le texte va passer à la ligne et ne dépassera pas la largeur disponible. Par contre, sur les écrans larges, les lignes du texte peuvent devenir trop longues, ce qui gênera la lisibilité.

### Approche "classique"

```css
.container {
  /* ici la largeur ne dépassera pas  640px */
  max-width: 35rem;
  /* et si besoin de centrer ce contenu */
  margin-left: auto;
  margin-right: auto;
}
```

https://codepen.io/alyra/pen/ZEBMwgB

### Unité `ch`

Selon les bonnes pratique de la lisibilité, une colonne idéale devrait contenir 50 à 75 caractères par ligne (environ 8 à 10 mots en anglais). Nous devons veiller à ne pas nous éloigner trop des ces valeurs.

L'unité qui représente la largeur du caractère de la police est : `ch`. Plus précisement, cette unité représente la largeur du caractère « 0 » (zéro).

[Readability: the Optimal Line Length](https://baymard.com/blog/line-length-readability)

```css
.text-container {
  max-width: 60ch;
  /* et si besoin de centrer ce contenu */
  margin-left: auto;
  margin-right: auto;
}
```

https://codepen.io/alyra/pen/KKNxqym

## Media queries

Media queries marchent comme des filtres appliqués à CSS. Ils permettent d'écrire les déclarations CSS qui ne sont prises un compte que dans certains situations. Par exemple, nous pouvons mettre en place des styles qui seront pris en compte uniquement si notre document est imprimé.

[media queries sur MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)

```css
@media print {
  /* les style pour print viennent ici */
  nav {
    display: none;
  }
  a[href]:after {
    content: " (" attr(href) ")";
  }
}
```

Nous pouvons utiliser media queries en fonction de la taille de l'écran, les dimension de l'appareil, son orientation, le support pour l'interactivité `hover`, etc.

```css
body {
  padding: 1rem;
}
@media (min-width: 800px) {
  body {
    padding: 2rem;
  }
}
/* la largeur du viewport minimum 1200px */
@media (min-width: 1200px) {
  body {
    border: 1rem solid red;
    padding: 3rem;
  }
}
```

```css
/* la largeur du viewport maximum 400px */
@media (max-width: 400px) {
  h1 {
    font-size: 1.5rem;
  }
}
```

## Hover

Il est important de savoir que les styles en état `:hover` ne fonctionnent pas tout à fait correctement sur les écrans tactiles (en particulier sur les écrans mobiles).

Parfois, manque de support pour `hover` peut rendre notre contenu inaccessible.

Heureusement, avec _media query_, il est possible de détecter le support pour `hover` et adapter nos styles :

```css
@media (hover: hover) {
  div {
    filter: blur(10px);
  }
  div:hover {
    filter: none;
  }
}
```

[@media hover sur MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/hover)

## Layout avec Bootstrap

```html
<!-- Bootstrap CSS -->
<link
  href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css"
  rel="stylesheet"
  integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl"
  crossorigin="anonymous"
/>
```

![grid](https://assets.codepen.io/4515922/BlogArticle-BootstrapGrid.png)

<table class="table" style="width: 100%">
  <thead>
    <tr>
      <th>Breakpoint</th>
      <th>Class infix</th>
      <th>Dimensions</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>X-Small</td>
      <td><em>None</em></td>
      <td>0–576px</td>
    </tr>
    <tr>
      <td>Small</td>
      <td><code>sm</code></td>
      <td>≥576px</td>
    </tr>
    <tr>
      <td>Medium</td>
      <td><code>md</code></td>
      <td>≥768px</td>
    </tr>
    <tr>
      <td>Large</td>
      <td><code>lg</code></td>
      <td>≥992px</td>
    </tr>
    <tr>
      <td>Extra large</td>
      <td><code>xl</code></td>
      <td>≥1200px</td>
    </tr>
    <tr>
      <td>Extra extra large</td>
      <td><code>xxl</code></td>
      <td>≥1400px</td>
    </tr>
  </tbody>
</table>

```html
<div class="container">
  <div class="row">
    <div class="col-md">Column</div>
    <div class="col-md">Column</div>
  </div>
</div>
```

https://codepen.io/alyra/pen/OJboqOP

https://codepen.io/alyra/pen/VwmGRyR

---

## Exercices

- “Migrer” Hello Marie dans VSCODE
- [Chotto Motto](https://github.com/pehaa/hardfork-ex-chotto-motto)
- [B5 - responsive grid](https://codepen.io/alyra/pen/ExPZwWK)
