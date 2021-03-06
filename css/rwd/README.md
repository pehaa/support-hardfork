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

Selon les bonnes pratique de la lisibilité, une colonne idéale devrait contenir 70 à 80 caractères par ligne (environ 8 à 10 mots en anglais). Nous devons veiller à ne pas nous éloigner trop des ces valeurs.

Voici un exemple qui prend en compte ces contraints :

```css
.container {
  /* ici la largeur ne dépassera pas  640px */
  max-width: 35rem;
  /* et si besoin de centrer ce contenu */
  margin-left: auto;
  margin-right: auto;
}
```

https://codepen.io/alyra/pen/VwKLRWd

## Media queries

Media queries marchent comme des filtres appliqués à CSS. Ils permettent d'écrire les déclarations CSS qui ne sont prises un compte que dans certains situations. Par exemple, nous pouvons mettre en place des styles qui seront pris en compte uniquement si notre document est imprimé.

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
/* la largeur du viewport minimum 1200px */
@media (min-width: 1200px) {
  body {
    border: 3rem solid white;
  }
}
```

```css
/* la largeur du viewport maximum 400px */
@media (max-width: 400px) {
  body {
    padding: 1rem;
  }
}
```

## Hover

Il est important de savoir que les styles en état `:hover` ne fonctionnent pas tout à fait correctement sur les écrans tactiles (en particulier sur les écrans mobiles).

Parfois, manque de support pour `hover` peut rendre notre contenu inaccessible (comme dans notre exemple [Hello Marie](https://codepen.io/alyra/pen/ZEQYYEr)).

Heureusement, avec *media query*, il est possible de détecter le support pour `hover` et adapter nos styles :

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

---

## Live coding

https://codepen.io/alyra/pen/ZEQYYEr

https://codepen.io/alyra/pen/88d72ae8d0d2618620106b56c2bfbc52

---

## Exercices

 - [Alyra](https://codepen.io/alyra/pen/LYGEyaL) | [solution](https://codepen.io/alyra/pen/996c8dcb84a663435a2da59232300985)
 - “Migrer” Hello Marie dans codeSandBox
