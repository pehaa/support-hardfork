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

Dans notre cas

```bash
├── src
│   ├── theme
│   │   ├── index.js
│   │   ├── styles.js
│   │   ├── foundations
│   │   │   └── fonts.js
│   │   ├── components
│   │   │   └── Badge.js
```

```js
// src/theme/index.js
import { extendTheme } from "@chakra-ui/react"
// Global style overrides
import { styles } from "./styles"
// Foundational style overrides
import { fonts } from "./foundations/fonts"
// Component style overrides
import Badge from "./components/Badge"
const overrides = {
  styles,
  fonts,
  // Other foundational style overrides go here
  components: {
    Badge,
    // Other components go here
  },
}
export default extendTheme(overrides)
```

```js
// src/theme/components/Badge.js
export const Badge = {
  baseStyle: {
    mb: 6,
    mx: "auto",
    borderRadius: "2xl",
    px: 2,
    py: 1,
  },
}
```

```js
// src/theme/foundations/fonts.js
export const fonts = {
  body: "Lato, sans-serif",
  headings: "Lato, sans-serif",
  special: "Trocchi, serif",
}
```

```js
export const styles = {
  global: {
    "html, body": {
      color: "gray.700",
    },
  },
}
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

## Mobile Navigation

Nous allons utiliser `useMediaQuery` - un hook de Chakra UI pour afficher le menu de navigion selon la taille de l'écran.

```js
import { Link, List, ListItem } from "@chakra-ui/react"
const NavigationListItems = ({ sx, ...props }) => {
  return (
    <List sx={{ ...sx }} {...props}>
      <ListItem>
        <Link href="/#sample">Sample</Link>
      </ListItem>
      <ListItem>
        <Link href="/#pricing">Pricing</Link>
      </ListItem>
      <ListItem>
        <Link href="/#buy-now">Buy now</Link>
      </ListItem>
    </List>
  )
}
```

```js
const Navigation = () => {
  const [isMobile] = useMediaQuery("(max-width: 720px)")
  return (
    <>
      <Box py="3" position="fixed" w="100%" zIndex="sticky">
        <Container as="nav" maxW="container.lg" d="flex" alignItems="center">
          <Link href="/" mr="auto">
            AlyraKit
          </Link>
          {isMobile ? (
            <MobileNavigation>
              <NavigationListItems />
            </MobileNavigation>
          ) : (
            <NavigationListItems />
          )}
        </Container>
      </Box>
    </>
  )
}

export default Navigation
```

### Mobile Navigation avec `Fade`

```js
import { Box, Fade, Button, IconButton, useDisclosure } from "@chakra-ui/react"
import { HamburgerIcon } from "@chakra-ui/icons"
const MobileNavigation = ({ children }) => {
  const { isOpen, onToggle } = useDisclosure()

  return (
    <>
      <Button
        as={IconButton}
        aria-label="Open mobile menu"
        icon={<HamburgerIcon />}
        variant="outline"
        onClick={onToggle}
      >
        Click Me
      </Button>
      <Box position="absolute" right="0" left="0" top="100%">
        <Fade in={isOpen} unmountOnExit={true}>
          {children}
        </Fade>
      </Box>
    </>
  )
}
```

### Mobile Navigation avec `Drawer`

```js
import { useRef } from "react"
import {
  Drawer,
  DrawerBody,
  DrawerOverlay,
  DrawerContent,
  DrawerCloseButton,
  Button,
  useDisclosure,
} from "@chakra-ui/react"

const MobileNavigation = ({ children }) => {
  const { isOpen, onOpen, onClose } = useDisclosure()
  const btnRef = useRef()

  return (
    <>
      <Button ref={btnRef} colorScheme="teal" onClick={onOpen}>
        Open
      </Button>
      <Drawer
        isOpen={isOpen}
        placement="right"
        size="sm"
        onClose={onClose}
        finalFocusRef={btnRef}
      >
        <DrawerOverlay />
        <DrawerContent>
          <DrawerCloseButton />
          <DrawerBody>{children}</DrawerBody>
        </DrawerContent>
      </Drawer>
    </>
  )
}
```

## Color Mode

### Color Mode Switcher

```js
import { MoonIcon, SunIcon } from "@chakra-ui/icons"
import { useColorMode, IconButton } from "@chakra-ui/react"

const ColorModeSwitch = () => {
  const { colorMode, toggleColorMode } = useColorMode()
  return (
    <IconButton
      onClick={toggleColorMode}
      aria-label="Switch color mode"
      icon={colorMode === "light" ? <MoonIcon /> : <SunIcon />}
    />
  )
}
```

Dans les components nous allons utiliser ensuite le hook [`useColorModeValue`](https://chakra-ui.com/docs/features/color-mode#usecolormodevalue).

Dans le theme :

```js
// src/theme/styles.js
import { mode } from "@chakra-ui/theme-tools"
export const styles = {
  global: props => ({
    "html, body": {
      //color: props.colorMode === "light" ? "gray.800" : "white",
      //bg: props.colorMode === "light" ? "white" : "teal.800",
      color: mode("gray.800", "white")(props),
      bg: mode("white", "teal.800")(props),
    },
  }),
}
```

## Transition en scroll avec Chakra UI

Chakra UI vient avec [des transitions,](https://chakra-ui.com/docs/components/transitions) nous avons des Components `Fade`, `ScaleFade`, `Slide` et `SlideFade`.

Nous allons s'en servir et déclancher la transition au moment où un componentre entre dans le _viewport_ pour la premier fois. Pour détecter cela nous allons installer et utiliser [`react-in-viewport`] (npmjs.com/package/react-in-viewport) et son hook `useInViewPort`.

```bash
yarn add react-in-viewport
```

### Slide & FadeIn

```js
import { useRef } from "react"
import { SlideFade } from "@chakra-ui/transition"
import { useInViewport } from "react-in-viewport"

const SlideFadeOnScroll = ({ children, offsetY = "60px" }) => {
  const myRef = useRef()
  const { enterCount } = useInViewport(myRef, { threshold: 0.1 }, {}, {})
  return (
    <SlideFade
      ref={myRef}
      in={enterCount}
      offsetY={offsetY}
      delay={{ enter: 0.25 }}
      transition={{
        enter: { duration: 0.6 },
      }}
    >
      {children}
    </SlideFade>
  )
}

export default SlideFadeOnScroll
```

### Scale & FadeIn

```js
import { useRef } from "react"
import { ScaleFade } from "@chakra-ui/transition"
import { useInViewport } from "react-in-viewport"

const ScaleFadeOnScroll = ({ children, initialScale = 0.9 }) => {
  const myRef = useRef()
  const { enterCount } = useInViewport(myRef, { threshold: 0.1 }, {}, {})

  return (
    <ScaleFade
      ref={myRef}
      in={enterCount}
      initialScale={initialScale}
      delay={{ enter: 0.25 }}
      transition={{
        enter: { duration: 1 },
      }}
    >
      {children}
    </ScaleFade>
  )
}

export default ScaleFadeOnScroll
```

## Count Transition

Nous allons utiliser [React CountUp](https://www.npmjs.com/package/react-countup)

```bash
yarn add react-countup
```

**Avant :**

```js
// src/components/Pricing.js
const [price, setPrice] = useState(config.yearly)
const handleSwitchChange = e => {
  if (e.target.checked) {
    setPrice(config.monthly)
  } else {
    setPrice(config.yearly)
  }
}

// ...
return (
  // ...
  <Center>
    <Text as="b" fontSize="6xl">
      $ {price}
    </Text>{" "}
    /mo
  </Center>
  // ...
)
```

**Avec CountUp :**

```js
// src/components/Pricing.js
import { useCountUp } from "react-countup"
// ...
const { countUp, update } = useCountUp({
  start: config.yearly,
  end: config.monthly,
  delay: 0,
  startOnMount: false,
  duration: 0.6,
})
const handleSwitchChange = e => {
  if (e.target.checked) {
    update(config.monthly)
  } else {
    update(config.yearly)
  }
}
// ...
return (
  // ...
  <Center>
    <Text as="b" fontSize="6xl">
      $ {countUp}
    </Text>{" "}
    /mo
  </Center>
  // ...
)
```
