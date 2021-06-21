# CSS-in-JS avec Chakra UI

Sous le capot Chakra UI utilise

- [Emotion](https://emotion.sh/docs/introduction)
- [styled-system](https://styled-system.com/)

[CodeSandbox - rapide exemple avec Emotion](https://codesandbox.io/s/weathered-frog-eby06)

Voici notre [repo de départ](https://github.com/pehaa/alyrakit), nous allons créer une landing page [comme celle-ci.](https://alyrakit.netlify.app/)

## Installation

```bash
yarn add @chakra-ui/react @emotion/react@^11 @emotion/styled@^11 framer-motion@^4
```

## `ChakraProvider`

Nous allons enrober toute l'application dans `ChakraProvider`. Si nous utilisons CRA, nous pouvons le faire soit dans `src/index.js` ou `src/App.js`

```js
import React from "react"
import ReactDOM from "react-dom"
import App from "./App"
import reportWebVitals from "./reportWebVitals"
import { ChakraProvider } from "@chakra-ui/react"

ReactDOM.render(
  <React.StrictMode>
    <ChakraProvider>
      <App />
    </ChakraProvider>
  </React.StrictMode>,
  document.getElementById("root")
)

// comme avant
```

## Default Theme

`ChakraProvider` nous donne accès à un objet `theme` qui vient avec une "quantifications" des propriétés CSS. C'est utile de se familiariser des "token" qui sont disponible pour différentes propriétés [Docs - Default Theme](https://chakra-ui.com/docs/theming/theme)

## Déclarion des styles avec Style Props

Avec Ckakra UI nous ajoutons les styles principalement via [Style props](https://chakra-ui.com/docs/features/style-props).

```js
<Box mb={4} px={3}>
  Hello!
</Box>
// margin-bottom: 1rem;
// padding-left: 0.75rem;
// padding-right: 0.75rem;
```

attention, ceci ne marchera pas :

```js
// 👎
<div mb={4}>Hello!</div>
```

Nous devons passer les _styles props_ aux components Chakra

## Déclarion des styles avec la props `sx`

```js
<Box
  as="nav"
  sx={{ a: { textDecoration: "none", textTransform: "uppercase" } }}
>
  <List>
    <ListItem>
      <Link>Link 1</Link>
    </ListItem>
    <ListItem>
      <Link>Link 2</Link>
    </ListItem>
  </List>
</Box>
```

## Footer

```js
import { Box } from "@chakra-ui/react"

const Footer = () => {
  return (
    <Box as="footer" bg="gray.800" color="white" p="4" textAlign="center">
      Built with Chakra UI
    </Box>
  )
}

export default Footer
```

## Theme - global styles & overrides

```bash
├── src
│   ├── theme
│   │   ├── index.js
│   │   ├── styles.js
│   │   ├── foundations
│   │   │   ├── ...
│   │   │   └── ...
│   │   ├── components
│   │   │   ├── ...
│   │   │   └── ...
```

```js
// src/theme/index.js
import { extendTheme } from "@chakra-ui/react"
// Global style overrides
import styles from "./styles"
// Foundational style overrides
import borders from "./foundations/borders"
// Component style overrides
import Button from "./components/button"
const overrides = {
  styles,
  borders,
  // Other foundational style overrides go here
  components: {
    Button,
    // Other components go here
  },
}
export default extendTheme(overrides)
```

et ensuite

```js
// src/index.js
import React from "react"
import ReactDOM from "react-dom"
import App from "./App"
import reportWebVitals from "./reportWebVitals"
import { ChakraProvider } from "@chakra-ui/react"
import theme from "./theme"

ReactDOM.render(
  <React.StrictMode>
    <ChakraProvider theme={theme}>
      <App />
    </ChakraProvider>
  </React.StrictMode>,
  document.getElementById("root")
)

// comme avant
```
