# Todos avec une base de donnée et l'API

## Mise en place du backend

Clonez [ce repo](https://github.com/pehaa/fake-api)

```bash
cd fake-api
yarn
yarn start
```

Nous allons pouvoir communiquer avec une base de donnée (`database.json`) via une API Rest. Nous allons tester notre API avec [Hoppscotch](https://hoppscotch.io/)

### `GET`

```js
fetch("http://localhost:4000/todos", {
  method: "GET" // default
})
```

### `POST`

```js
fetch("http://localhost:4000/todos", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({

  })
})
```


### `PUT`

```js
fetch("http://localhost:4000/todos", {
  method: "PUT",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    
  })
})
```


### `DELETE`

```js
fetch("http://localhost:4000/todos", {
  method: "DELETE",
  headers: {
    "Content-Type": "application/json"
  }
})
```


, `POST`, `DELETE`, `PUT`