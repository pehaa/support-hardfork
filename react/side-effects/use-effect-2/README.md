# Fetching data in React

## Fetch API

Nous allons utiliser l'API `Fetch`, disponible nativement dans le navigateur. Dans nos projets web nous n'avons pas besoin d'installer _axios_.

L'api `Fetch` nous fournit la méthode `fetch` qui permet de récupérer des ressources à travers le réseau de manière asynchrone (comme `axios.get`).

- `fetch` retourne une promise
- la promise retournée par fetch _resolves_ avec `response` qui ne contient pas encore des données, c'est une réponse générique (ok, status, ...)
- pour obtenir les données nous allons appeler `response.json()` qui aussi retourne une promise
- la promesse retournée par fetch () ne _rejects_ pas si le status de la réponse HTTP est 40x ou 50x. Par contre elle _resolves_ en `response` avec `ok: false`

```javascript
fetch("https://css-tricks.com/wp-json/wp/v2/posts")
  .then((response) => {
    if (!response.ok) {
      throw new Error("something went wrong")
    }
    return response.json()
  })
  .then((data) => {
    console.log(data)
  })
  .catch((error) => {
    console.error(error.message)
  })
```

## `fetch` dans `useEffect`

```js
useEffect(() => {
  fetch("https://myfake.api")
    .then((response) => {
      if (!response.ok) {
        throw new Error("something went wrong")
      }
      return response.json()
    })
    .then((data) => {
      console.log(data)
    })
    .catch((error) => {
      console.error(error.message)
    })
}, [])
```

```js
useEffect(() => {
  fetch(`https://myfake.api?param1=${myParam1}&param2=${myParam2}`)
    .then((response) => {
      if (!response.ok) {
        throw new Error("something went wrong")
      }
      return response.json()
    })
    .then((data) => {
      console.log(data)
    })
    .catch((error) => {
      console.error(error.message)
    })
}, [myParam1, myParam2])
```

https://codepen.io/alyra/pen/OJWqmmv

```js
const Blog = () => {
  const [posts, setPosts] = useState([])
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState("")
  React.useEffect(() => {
    setLoading(true)
    fetch("https://css-tricks.com/wp-json/wp/v2d/posts")
      .then((response) => {
        if (!response.ok) {
          throw new Error(
            `Quelque chose n'a pas marché, status ${response.status}`
          )
        }
        return response.json()
      })
      .then((data) => {
        console.log(data)
        setPosts(data)
      })
      .catch((error) => {
        setError(error.message)
      })
      .finally(() => {
        setLoading(false)
      })
  }, [])

  return (
    <section className="container">
      <h1>Derniers Articles de CSS-Tricks</h1>
      {loading && <p>Loading....</p>}
      {error && <p>{error}</p>}
      {posts.map((post) => {
        return (
          <article key={post.id}>
            <h2
              dangerouslySetInnerHTML={{
                __html: post.title.rendered
              }}
            />
            <div
              dangerouslySetInnerHTML={{
                __html: post.excerpt.rendered
              }}
            />
          </article>
        )
      })}
    </section>
  )
}
```

### Syntaxe `async` / `await`

https://codepen.io/alyra/pen/mdRomQx

## Weather App

- [Demo](https://hardfork-weather.netlify.app/)
- [Starter repo](https://github.com/pehaa/alyra-weather-app-start)
- [fetch](https://codepen.io/alyra/pen/eYgXbdV)
- [Preview data React](https://codepen.io/alyra/pen/oNBVJxm)



## Exercices :

- Créer CRA et reproduire [Starwars Planets](https://stupefied-wozniak-d85a53.netlify.app/) - toute créativité est bienvenue