# Fetching data in React

## Fetch API

Nous allons utiliser une API `Fetch`, disponible nativement dans le navigateur - pas besoin d'installer des bibliothèque (comme axios par exemple).

L'api Fetch nous fournit la méthode `fetch` qui permet de récupérer des ressources à travers le réseau de manière asynchrone (comme `axios.get`).

- `fetch` retourne une promise
- la promise retournée par fetch _resolves_ avec `response`, `response` ne contient pas encore des données, c'est une réponse générique (ok, status, ...)
- pour obtenir les données nous allons appeler `response.json()` qui aussi retourne une promise
- La promesse retournée par fetch () ne _rejects_ pas si le status de la réponse HTTP est 40x ou 50x. Par contre elle _resolves_ en `response` avec `ok: false`

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

##

## Exercices :

-
