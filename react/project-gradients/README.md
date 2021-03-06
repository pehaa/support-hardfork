# Projet Gradients

Dans ce challenge vous avez pour but de construire une application web _monopage_ [comme celle-ci.](https://alyra-gradients-bonus.netlify.app/)

**Vous devez utiliser [ce repo](https://github.com/pehaa/gradients-project-start) pour démarrer** (fork, clone & yarn). Ci-dessous, vous trouverez quelques indications qui vous aideront à structurer votre projet.

## CRA - configuration initiale

- Bootstrap est déjà installé et intégré (`./src/index.js`)
- La partie "meta" (title, lang, etc.) est déjà mise en place (`./public/index.html`)
- Le fichier `./src/gradient.js` exporte deux variables `gradients` et `uniqueTags`.

  ```js
  export const gradients = [
    {
      name: "Grade Grey",
      start: "rgb(189, 195, 199)",
      end: "rgb(44, 62, 80)",
      tags: ["gris"],
    },
    {
      name: "Harvey",
      start: "rgb(31, 64, 55)",
      end: "rgb(153, 242, 200)",
      tags: ["vert"],
    },
    {
      name: "Rainbow Blue",
      start: "rgb(0, 242, 96)",
      end: "rgb(5, 117, 230)",
      tags: ["vert", "bleu"],
    },
    ...
  ]
  ```

---

- Structure initiale de projet

```bash
src
├── App.js
├── components
│   ├── Footer.js
│   ├── Gradient.js
│   ├── GradientCode.js
│   ├── GradientPill.js
│   ├── GradientTitle.js
│   ├── GradientsApp.js
│   ├── GradientsList.js
│   └── GradientsSelect.js
├── gradients.js
...
```

## À faire :

![](https://wptemplates.pehaa.com/assets/alyra/gradients-app.png)

![](https://wptemplates.pehaa.com/assets/alyra/gradient.png)

En respectant la structure des components comme ci-dessus, réaliser les étapes suivantes :

1. Afficher tous les gradients (25).

<details>
  <summary>Comment ? 🤔 (cliquer ici pour quelques astuces)</summary>
  <p>✨ Pour ceci il faudrait importer la variable <code>gradient</code> depuis <code>./src/gradients.js</code> et la parcourir avec la méthode <code>map</code>.</p>
  <p>✨ Vous pouvez utiliser la propriété <code>name</code> pour l'attribut <code>key</code>.</p>
</details>

---

2. Modifier le component `Gradient`. Ajouter les boutons pour les tags de chaque dégradé.

![](https://wptemplates.pehaa.com/assets/alyra/gradients-tags.png)

---

3. Ajouter le component `GradientsSelect`. Il devrait contenir un élément `<select>` qui permettera de filtrer les dégradés par tag ("tous", "gris", "vert", ...).

<details>
  <summary>Comment ? 🤔</summary>
  <p>✨ Vous devez importer la variable <code>uniqueTags</code> depuis <code>./src/gradients.js</code></p>
  <p>🕵 N'hésitez pas à "inspecter l'élément" pour retrouver le markup de cette partie de la page.</p>
</details>

---

4. Ajouter le component `GradientsHeader`

<details>
  <summary>🤔</summary>
  <p>✨ Vous pouvez vous servir de <a href="https://codepen.io/alyra/pen/rNepaOy" target="_blank" rel="noopener">ce pen.</a></p>
</details>

---

5. Mettre en place la fonctionnalité **"filtrer par tag".**

<details>
  <summary>🤔</summary>
  <p>✨ Ce qui définit le <i>state</i> de notre application est la valeur de filtre (le tag choisi). Ce pourait être alors
  <code>const [filter, useFilter] = React.state("tous")</code>
  </p>
</details>

---

## Validation (/10):

**[le site de référence](https://alyra-gradients-bonus.netlify.app/)**

- rendu sur GitHub et déploiement sur Netlify
- la structure des components est respectée (/1)
- le design est respecté (/1)
- l'ensemble des dégradés sont affichés correctement dans la page (/1)
- le header est fonctionnel (/3)
  - le bouton _"refresh"_ est fonctionnel (1/3)
  - les boutons _"next/prev"_ sont fonctionnels (2/3)
- le select est fonctionnel et permet de filter des dégradés par tag (/2)
- les boutons sont fonctionnels et permettent de filter des dégradés par tag (/1)
- il n'y a pas de warning depuis la console dans VSCode, ni dans la console du navigateur (/1)
- [le html est valide (validator w3c)](https://validator.w3.org/nu/)
