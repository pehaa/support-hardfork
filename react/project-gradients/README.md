# Projet Gradients

Dans ce challenge vous avez pour but de construire une application web _monopage_ [comme celle-ci.](https://alyra-gradients-bonus.netlify.app/)

Vous pouvez utiliser [ce repo]() pour démarrer (fork, clone & yarn). Ci-dessous, vous trouverez quelques indications qui vous aideront à structurer votre projet.

## CRA - configuration initiale

- Bootstrap est déjà installé est intégré (`./index.js`)
- La partie "meta" (title, lang, etc.) est déjà mise en place (`./public/index.html`)
- Le fichier `./gradient.js` exporte deux variables `gradients` et `uniqueTags`.
- Structure initialle de projet

## À faire :

1. Afficher tous les gradients. 

<details>
  <summary>Comment ? 🤔 (cliquer ici pour quelques astuces)</summary>
  <p>✨ Pour ceci il faurait importer la variable <code>gradient</code> et la parcourir avec la méthode <code>map</code> Vou pouvez utiliser la propriété <code>name</code> pour l'attribut <code>key</code> </p>
</details>

---

2. Modifier le component `Gradient`. Ajouter les boutons pour les tags de chaque dégradé.

![](https://wptemplates.pehaa.com/assets/alyra/gradients-tags.png)

---

3. Ajouter le component `GradientsSelect`. Il devrait contenir un élément `<select>` qui permettera de filtrer les dégradés par tag ('tous', 'gris', 'vers', ...).

<details>
  <summary>Comment ? 🤔 (cliquer ici pour quelques astuces)</summary>
  <p>✨ Vous pouvez importer la variable <code></code> depuis <code>./gradients.js</code></p>
  <p>N'hésitez pas à "inspecter élément" pour retrouver le markup correct.</p>
</details>

--- 

4. Ajouter le component `GradientsHeader`

<details>
  <summary>🤔 (cliquer ici pour quelques astuces)</summary>
  <p>✨ Vous pouvez vous servir de <a href="https://codepen.io/alyra/pen/rNepaOy" target="_blank" rel="noopenr">ce pen.</a></p>
</details>

---

5. Mettre en place la possibilité de filtrer par tag.



## Structure du projet

Mettez en place un dossier `src/components` avec la structure comme ceci. C’est une structure recommandée, mais elle peut varier un peu 😉.

```bash
src
├── App.js
├── App.test.js
├── components
│   ├── Footer.js
│   ├── Gradient.js
│   ├── GradientCode.js
│   ├── GradientPill.js
│   ├── GradientTags.js
│   ├── GradientTitle.js
│   ├── Gradients.js # (**)
│   ├── GradientsHeader.js
│   ├── GradientsList.js
│   └── GradientsSelect.js
├── gradients.js
├── index.css
├── index.js
├── serviceWorker.js
└── setupTests.js
```

(`*`) - contient le component `Gradient`  
(`**`) - component parent, contient `GradientsList` et `GradientsSelect`

---

**Vous pouvez voir le site de référence [ici](https://alyra-gradients-bonus.netlify.app/)**

---

## Validation :

- rendu sur GitHub et déploiement sur Netlify
- la structure est respecté (/2)
- tous les dégradés sont affichés dans la page
- le header est fonctionnel (/3)
  - refresh est fonctionnel (1/3)
  - next/prev fonctionnels (2/3)
- le select fonctionnel et permet de filter des dégradés par tag (/1)
- les boutons sont fonctionnels et permettent de filter des dégradés par tag (/1)
- il n'y a pas de warning depuis la console dans VSCode, ni dans la console du navigateur (/2)
- [le html est valide (validator w3c)](https://validator.w3.org/nu/)
