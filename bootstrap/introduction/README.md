# Bootstrap 5

Bootstrap est une librairie open source qui unifie et accélère la mise en page. C'est une de bibliothèque CSS la plus connue et la plus utilisée (source [The State of CSS 2020](https://2020.stateofcss.com/en-US/technologies/css-frameworks/))

![](https://wptemplates.pehaa.com/assets/alyra/usage.png)

![](https://wptemplates.pehaa.com/assets/alyra/awareness.png)

Comme avec n'importe quelle bibliothèque, il est indispensable d'apprendre se servir de sa documentation. Vous trouverez la documentation de bootstrap sur [le site officiel de Bootstrap 5.](https://v5.getbootstrap.com/)

## Comment s'en servir ?

Bootstrap vient avec un grand fichier <code>.css</code> déjà écrit.
Ceci dit, la grande partie de la mise en page se passera dans les
fichiers <code>.html</code> où nous appliquerons des **classes** définies
dans le stylesheet de bootstrap.

Il faut un peu de temps et de la pratique pour apprivoiser cette
approche. Mais, attention, c'est un bon investissement, puisque c'est
une approche qui <b>accélère</b> le processus de développement.

Nous devons alors en même temps apprendre à nous servir de [la documentation](https://v5.getbootstrap.com/docs/5.0/getting-started/introduction/)
et comprendre la logique des noms des classes.

## Que fait le fichier css de bootstrap ?

- met en place une solution basée sur `normalize.css`
- met en place `* {box-sizing: border-box;}`
- expose un grand nombre des classes css, prêtes à utiliser, en particulier concernant
  - des couleurs
    ![](https://wptemplates.pehaa.com/assets/alyra/colors-b5.png)
  - des styles typographiques (alignement du texte, tailles, styles, graisse, des polices, la casse, l'interligne. etc.)
    ![](https://wptemplates.pehaa.com/assets/alyra/typo-b5.png)
  - espacements (paggings et margin)
  - layout (le système de la grille basé sur le module _flexbox_)
  - formulaires
  - barre de navigation (responsive avec JavaScript)

Si vous vous rappelez de [cet exercice](https://codepen.io/alyra/details/VwvRzRJ), c'est le même concept, sauf que le nombre des classes css est beaucoup plus important...

Et si cela vous intéresse, voici [le coupable](https://codepen.io/alyra/pen/eeebfe01507653f20f9193f3ae5cf1ea.css) - attention, il contient environ 10k lignes 🤯.

## Mise en place

**Starter template**

Le code ci-dessous correspond à la version de bootstrap 5 publiée au moment de l'écriture de ce document. Les liens vers les fichiers sources sont probablement différents, mais vous trouverez la version à jour dans la documentation officielle :).

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />

    <!-- Bootstrap CSS -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl"
      crossorigin="anonymous"
    />

    <title>Hello, world!</title>
  </head>

  <body>
    <h1>Hello, world!</h1>
    <!-- ... -->
    <!-- Optional JavaScript (pour qqs components en particulier) -->
    <script
      src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.bundle.min.js"
      integrity="sha384-b5kHyXgcpbZJO/tY9Ul7kGkf1S0CWuKcCD38l8YkeH8z8QjE0GmW1gYU5S9FOnJ0"
      crossorigin="anonymous"
    ></script>
  </body>
</html>
```

### Couleurs 🌈

Bootstrap met en disposition des classes avec le prefix `bg-` qui modifie la propriété `background-color`. Nous avons à notre disposition :

- `bg-primary`
- `bg-secondary`
- `bg-success`
- `bg-danger`
- `bg-warning`
- `bg-info`
- `bg-light`
- `bg-dark`
- `bg-transparent`

Pour changer la propriété `color` nous allons utiliser les classes avec préfix `text` :

- `text-primary`
- `text-secondary`
- `text-success`
- `text-danger`
- `text-warning`
- `text-info`
- `text-light`
- `text-dark`
- `text-body`
- `text-muted`
- `text-white`
- `text-black-50`
- `text-white-50`

ou (si ceci concerne les liens) :

![](https://wptemplates.pehaa.com/assets/alyra/links-b5.png)

```html
<a href="#" class="link-primary">Primary link</a>
<a href="#" class="link-secondary">Secondary link</a>
<a href="#" class="link-success">Success link</a>
<a href="#" class="link-danger">Danger link</a>
<a href="#" class="link-warning">Warning link</a>
<a href="#" class="link-info">Info link</a>
<a href="#" class="link-light">Light link</a>
<a href="#" class="link-dark">Dark link</a>
```

[Source : Docs > Utilities > Colors.](https://getbootstrap.com/docs/5.0/utilities/colors/)

## Espacements & Layout

### Breakpoints

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

Nous pouvons appliquer des classes <code>p..-..</code> et
<code>m..-..</code> afin d'appliquer des paddings et des margins prédéfinis par bootstrap.

```html
<p class="m-0">Je suis un paragraphe et je mets mes marges à zéro</p>
```

```html
<section class="p-4">
  Je suis une section et je mets des paddings de 1.5rem.
</section>
```

```html
<section class="p-md-5">
  Je suis une section et je quand l'écran dépasse 768px je mets des paddings de
  3rem.
</section>
```

```html
<section class="p-3 p-md-5">
  Je suis une section je mets des paddings de 1rem mais quand l'écran dépasse
  768px je les change pour de 3rem.
</section>
```

https://codepen.io/alyra/pen/oNzbyKL

[Source : Docs > Utilities > Spacing](https://getbootstrap.com/docs/5.0/utilities/spacing/)

**Syntaxe :** `p{side}-{breakpoint}-{size}`  
**Valeurs :** `0`, `0.25rem`, `.5rem`, `1rem`, `1.5rem`, `3rem`

<dl>
  <dt><code>p-0</code></dt>
  <dd><code>.p-0 {padding: 0;}</code></dd>
  <dt><code>p-1</code></dt>
  <dd><code>.p-1 {padding: .25rem;}</code></dd>
  <dt><code>p-2</code></dt>
  <dd><code>.p-2 {padding: .5rem;}</code></dd>
  <dt><code>p-3</code></dt>
  <dd><code>.p-3 {padding: 1rem;}</code></dd>
  <dt><code>p-4</code></dt>
  <dd><code>.p-4 {padding: 1.5rem;}</code></dd>
  <dt><code>p-5</code></dt>
  <dd><code>.p-5 {padding: 3rem;}</code></dd>
</dl>
<dl>
  <dt><code>pt-0</code></dt>
  <dd><code>.pt-0 {padding-top: 0;}</code></dd>
  <dt><code>pe-1</code></dt>
  <dd><code>.pe-1 {padding-right: .25rem;}</code></dd>
  <dt><code>pb-2</code></dt>
  <dd><code>.pb-2 {padding-bottom: .5rem;}</code></dd>
  <dt><code>ps-3</code></dt>
  <dd><code>.ps-3 {padding-left: 1rem;}</code></dd>
  <dt><code>px-4</code></dt>
  <dd>
    <code>.px-4 {padding-left: 1.5rem; padding-right: 1.5rem;}</code>
  </dd>
  <dt><code>py-5</code></dt>
  <dd>
    <code>.py-5 {padding-top: 3rem; padding-bottom: 3rem;}</code>
  </dd>
</dl>

Le même système s'applique aux margins, avec quelques classes en
plus <code>m{sides}-auto</code>

<dl>
  <dt><code>m-auto</code></dt>
  <dd><code>.m-auto {margin: auto;}</code></dd>
  <dt><code>mt-auto</code></dt>
  <dd><code>.mt-auto {margin-top: auto;}</code></dd>
  <dt><code>ms-auto</code></dt>
  <dd><code>.ms-auto {margin-left: auto;}</code></dd>
  <dt><code>me-auto</code></dt>
  <dd><code>.me-auto {margin-right: auto;}</code></dd>
  <dt><code>mx-auto</code></dt>
  <dd><code>.mx-auto {margin-left: auto; margin-right: auto;}</code></dd>
  <dt><code>my-auto</code></dt>
 <dd><code>.my-auto {margin-top: auto; margin-bottom: auto;}</code></dd>
</dl>

### Système de grid bootstrap

#### Container

https://codepen.io/alyra/pen/dyGWJjx

#### Grille

La grille de bootstrap est, comme la plupart des systèmes de grille, basée sur **12** colonnes.

![grid](https://assets.codepen.io/4515922/BlogArticle-BootstrapGrid.png)

Les noms de classes correspondent au nombre de colonnes occupées.

```html
<div class="container">
  <div class="row">
    <div class="col-6">Je prends la place de 6 colonnes sur 12</div>
    <div class="col-6">Je prends la place de 6 colonnes sur 12</div>
  </div>
</div>
```

https://codepen.io/alyra/pen/dyGWJjx

https://codepen.io/alyra/pen/OJboqOP

https://codepen.io/alyra/pen/VwmGRyR

https://codepen.io/alyra/pen/OJbaZem

### Bootstrap Icons

[Documentation](https://icons.getbootstrap.com/)

---

[Exemples officiels Bootstrap](https://getbootstrap.com/docs/5.0/examples/)

## Exercices

- [B5 - text](https://codepen.io/alyra/pen/GRorKox)
- [B5 - m/p/colors](https://codepen.io/alyra/pen/RwrKZzg)
- [B5 - responsive grid](https://codepen.io/alyra/pen/jOWLNOw)
