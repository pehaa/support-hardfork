# HTML - Quelques bonnes pratiques

## Headings - je ne saute pas les niveaux de titre.

Les balises `<h1>`, `<h2>`, &hellip; , `<h6>` devraient toujours correspondre à la hierarchie du document (sa structure logique).

Nous n'utilisons pas `h3` directement dans la partie avec heading `h1`, ni `h4` dans la partie avec un heading de `h2` et ainsi de suite.

## Navigation - j'utilise `nav` et une liste `ul` (`ol`)

```html
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/projects">Projects</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>
```

Dans le cas où le document contient plusieurs éléments `nav`, on peut utiliser l'attribut `aria-labelledby` couplé avec un élément heading par son attribut `id`. Ceci facilitera la navigation aux personnes utilisant les screen readers.

```html
<header>
  <nav aria-labelledby="primary-navigation">
    <h2 id="primary-navigation">Site navigation</h2>
    <ul>
      <li><a href="/">Home</a></li>
      <li><a href="/projects">Projects</a></li>
      <li><a href="/contact">Contact</a></li>
    </ul>
  </nav>
</header>

<!-- page content -->

<footer>
  <nav aria-labelledby="footer-navigation">
    <h2 id="footer-navigation">Find us on social networks</h2>
    <ul>
      <li><a href="#">Twitter</a></li>
      <li><a href="#">Facebook</a></li>
      <li><a href="#">Instagram</a></li>
    </ul>
  </nav>
</footer>
```

## Main - exactement "un" `main` par document

L’élément HTML `<main>` représente le contenu majoritaire et unique du `<body>` du document.

Dans le contexte de l'accessibilité, cette balise va être utilisées par les outils d'assistance afin d'identifier et de naviguer rapidement dans la partie principale du document.

```html
<body>
  <header>
    <!-- ... -->
    <nav>
      <!-- ... -->
    </nav>
  </header>
  <main>
    <!-- ... -->
  </main>
  <footer>
    <!-- ... -->
  </footer>
</body>
```

## Images - je n'utilise jamais une image sans l'attribut alt

```html
<img src="panda-roux.jpg" alt="Panda roux sur une branche" />
<img src="fioriture.svg" alt="" />
```

## Icônes dans liens et boutons - j'ajoute toujours le contenu textuel

```html
<button typr="button" aria-label="Close">x</button>
```

```html
<a href="https://twitter.com/pehaa" aria-label="Trouvez moi sur Twitter">
  <i class="fab fa-twitter"></i>
</a>
```

```css
.sr-only {
  clip: rect(0 0 0 0);
  clip-path: inset(100%);
  height: 1px;
  overflow: hidden;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}
```

```html
<button typr="button">
  <span class="sr-only">Close the pop-up window here</span>
  <span aria-hidden="true">x</span>
</button>
```

## Emojis

```html
<span role="img" aria-label="heart">💖</span>
<span role="img" aria-label="pizza">🍕</span>
```

## Labels & inputs

Chaque contrôle interactif (`input`) devrait être accompagné par un élément [`label`](https://developer.mozilla.org/fr/docs/Web/HTML/Element/label) qui le décrit. Pour que la liaison fonctionne l'attribut `for` de `<label>` doit correspondre à l'attribut `id` de `input`.

```html
<label for="user-name">Votre nom</label>
<input id="user-name" name="user-name" type="text" />
```

ou l'élément `input` est descendant de son élément `label`

```html
<label>Votre nom</label>
  <input id="user-name" name="user-name" type="text" />
</label>
```

### Ressources

- [MDN - Headings & Navigation](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements#navigation)
- [MDN - Référence des éléments HTML](https://developer.mozilla.org/fr/docs/Web/HTML/Element)
- [HTML 5.2 W3C Recommendation](https://www.w3.org/TR/html52/)
- [HTML Reference](https://htmlreference.io/)

---

## Exercices

- [Portfolio de Sonia](https://codepen.io/alyra/pen/PoPXqVy)
- [Jamais sans passer par Markup Validation W3c](https://codepen.io/alyra/pen/JjYaZZL)
- [Divitis](https://codepen.io/alyra/pen/yLYxEvJ)
- [Personal Landing Page](https://codepen.io/alyra/pen/WNQgyBw)
