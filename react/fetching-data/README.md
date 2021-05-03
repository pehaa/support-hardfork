# Fetching Data & what can go wrong?

Voici notre point de départ

```js
// src/App.js
```

```js
import { useState } from "react"
import Planet from "./Planet"

const Planets = () => {
  const [planets, setPlanets] = useState([])

  return (
    <>
      <div className="row">
        {planets.map(planet => {
          return <Planet key={planet.name} planet={planet} />
        })}
      </div>
    </>
  )
}

export default Planets
```

Notre component `Planet` est comme ceci :

```js
// src/components/Planet.js
const Planet = ({ planet }) => {
  const { name, population, climate } = planet
  return (
    <div className="col-md-6 col-lg-4 col-xl-3 mb-4">
      <article className="bg-warning p-3">
        <h2 className="h5">{name}</h2>
        <p className="mb-0">
          <b>population</b> <br /> {population}
        </p>
        <p className="mb-0">
          <b>climat</b> <br /> {climate}
        </p>
      </article>
    </div>
  )
}

export default Planet
```

## Step 1 - Fetch on mount

our commencer, nous allons afficher 10 premiers planètes, en utilisant cet URL `https://swapi.dev/api/planets/?page=1`

Nous allons utiliser

https://codepen.io/alyra/pen/yLgmyrg

## Step 2 - Loading

N'oublions pas que le temps d'attente pour la donnée peut varier. Nous devons informer l'utilisateur qu'il faudrait patienter.

<div style="width:100%;height:0;padding-bottom:38%;position:relative;"><iframe src="https://giphy.com/embed/GNVmMgSdY5nxK" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p></p>

Nous allons alors afficher `<p>loading...</p>` en attente.

Pour pouvoir "visionner" notre "loading" nous pouvons ralentir l'arrivée de notre résultat, par exemple comme ceci :

```js
fetch(...)
// ⚠️ à copier-coller es surtout NE PAS OUBLIER d'ENLEVER 😂
  .then(response => {
    return new Promise((resolved) => {
      setTimeout(() => resolved(response), 2000)
    })
  })
  .then(....)
```

alternativement dans la syntaxe `async/await`

```js
// ⚠️ à copier-coller es surtout NE PAS OUBLIER d'ENLEVER 😂
await new Promise(resolved => {
  setTimeout(() => resolved(), 2000)
})
```

https://codepen.io/alyra/pen/ExZqaBQ

## Step 3 - Affichage des erreurs

<div style="width:100%;height:0;padding-bottom:38%;position:relative;"><iframe src="https://giphy.com/embed/dvD0OAETRfCXC" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div>

Il est également important d'informer l'utilisateur si son action a échoué. Il est peu probable que l'utilisateur a son console ouverte - cette information devrait se trouver sur l'écran.

```js
<p className="alert alert-danger">Message d'erreur</p>
```

https://codepen.io/alyra/pen/gOgVpmp

## Step 4 - Give me more

<iframe src="https://giphy.com/embed/1jXGsHY2EKdL27mEMd" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p></p>

Nous allons mettre en place le Bouton **Suivantes**

https://codepen.io/alyra/pen/MWJNwmO

## Step 5 - _What can go wrong?_

<div style="width:100%;height:0;padding-bottom:43%;position:relative;"><iframe src="https://giphy.com/embed/uq6ILNBI6g3As" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p></p>

https://codepen.io/alyra/pen/dyNxoKx

## Step 6 - Unmounting components pendant le fetch

https://codepen.io/alyra/pen/ZELgGXr

## Step 7 - Aborting fetch

Nous allons utiliser [l'API `AbortController`](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)

https://codepen.io/alyra/pen/XWpvmxJ

```js
useEffect(() => {
  let isCancelled = false
  const controller = new AbortController()
  setLoading(true)
  fetch(url, {
    signal: controller.signal,
  })
    .then(response => {
      if (!response.ok) {
        throw new Error(`Something went wrong, status : ${response.status}`)
      }
      return response.json()
    })
    .then(data => {
      if (!isCancelled) {
        /* .... */
        setLoading(false)
      }
    })
    .catch(error => {
      if (!isCancelled) {
        setError(error.message)
        setLoading(false)
      }
    })
  return () => {
    isCancelled = true
    controller.abort()
  }
}, [url])
```
