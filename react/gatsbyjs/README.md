# Jamstack, SSR et GraphQL avec Gatsby JS

## Jamstack

- [Jamstack wtf (en)](https://jamstack.wtf/)
- [Jamstack wtf - explication en fr](https://jamstatic.fr/2019/02/07/c-est-quoi-la-jamstack/)

## SSR

Server side rendering, rendu côté serveur, le markup est généré en build - ceci n'est pas le cas de CRA.

> Render at build time, not at runtime

## GraphQL

GraphQL est une alternative à REST API. C'est un language de queries et un runtime:

- un seul endpoint
- deux opérations: query et mutation (pour modifier les données)
- retourne uniquement les champs demandés (optimisation en volumetrie des données)
- permet de _fetch_ plusieurs ressources dans une seule requête

- [GRAPHQL : UNE APPROCHE API PAS EN REST](https://www.technologies-ebusiness.com/enjeux-et-tendances/graphql-approche-api-rest)
- [GraphQL - documentation officielle](https://graphql.org/learn/)

https://codepen.io/pehaa/pen/GRWJMJO

https://codepen.io/pehaa/pen/zYZvwEG

## Gatsby JS

Un framework basé sur React et le concept SSR.

Pour commencer nous allons installer Gatsby CLI

```bash
npm install -g gatsby-cli
gatsby --version
```

Ensuite nous allons démarrer avec un des "starters" officiels

```bash
gatsby new my-gatsby-blog https://github.com/gatsbyjs/gatsby-starter-blog.git
```

et ensuite

```bash
cd my-blog-gatsby
yarn start
```

(`yarn start` appelle la commande `gatsby develop`).

## Notre todo :

1. Ajouter un nouvel article (`content/blog/xyx/index.md`)
2. Ajouter une page _About_ disponible sous `/about` ([tutorial Gatsby](https://www.gatsbyjs.com/docs/tutorial/part-2/))
   ```bash
   mkdir src/pages/about.js
   ```
3. Modifier la component `Bio`, remplacer le lien twitter pas le lien vers la page `about`
4. Modifier l'affichage de l'article - ajouter _Time to Read_
5. Ajouter un nouvel champs frontmatter, _featured_ (`true` ou `false`)
6. Ajouter un component `<FeaturedArticles />` qui listera 3 derniers articles qui sont _featured_
7. Intégrer Chakra UI

   ```bash
   yarn add @chakra-ui/gatsby-plugin @chakra-ui/react @emotion/react @emotion/styled framer-motion
   ```

   et ensuite

   ```js
   // gatsby-config.js
   module.exports = {
     plugins: [
       //  ....
       "@chakra-ui/gatsby-plugin",
     ],
   }
   ```

---

⚠️ Si vous rencontrez des erreurs de type `Can't resolve '../../public/page-data/sq/d/45754215878.json'`, vous devez supprimer le `.cache` de gatsby, vous pouvez le faire dans le terminal avec la commande

```bash
gatsby clean
```

et ensuite refaire `yarn start`

---
