## INTRODUCTION

Axios is a very popular JavaScript library you can use to perform HTTP requests, that works in both Browser and [Node.js](https://flaviocopes.com/node/) platforms.

It is promise-based, and this lets us write async/await code to perform [XHR](https://flaviocopes.com/xhr/) requests very easily.

Using Axios has quite a few advantages over the native [Fetch API](https://flaviocopes.com/fetch-api/):

- supports older browsers (Fetch needs a polyfill)
- has a way to abort a request
- has a way to set a response timeout
- has built-in CSRF protection
- supports upload progress
- performs automatic JSON data transformation
- works in Node.js

Axios can be installed using [npm](https://flaviocopes.com/npm/):

```
npm install axios
```

You can start an HTTP request from the `axios` object:

```
axios({
  url: 'https://dog.ceo/api/breeds/list/all',
  method: 'get',
  data: {
    foo: 'bar'
  }
})
```

but for convenience, you will generally use

- `axios.get()`
- `axios.post()`

## GET REQUESTS

One convenient way to use Axios is to use the modern (ES2017) async/await syntax.

This Node.js example queries the [Dog API](https://dog.ceo/) to retrieve a list of all the dogs breeds, using `axios.get()`, and it counts them:

```
const axios = require('axios')

const getBreeds = async () => {
  try {
    return await axios.get('https://dog.ceo/api/breeds/list/all')
  } catch (error) {
    console.error(error)
  }
}

const countBreeds = async () => {
  const breeds = await getBreeds()

  if (breeds.data.message) {
    console.log(`Got ${Object.entries(breeds.data.message).length} breeds`)
  }
}

countBreeds()
```

If you don’t want to use async/await you can use the [Promises](https://flaviocopes.com/javascript-promises/) syntax:

```
const axios = require('axios')

const getBreeds = () => {
  try {
    return axios.get('https://dog.ceo/api/breeds/list/all')
  } catch (error) {
    console.error(error)
  }
}

const countBreeds = async () => {
  const breeds = getBreeds()
    .then(response => {
      if (response.data.message) {
        console.log(
          `Got ${Object.entries(response.data.message).length} breeds`
        )
      }
    })
    .catch(error => {
      console.log(error)
    })
}

countBreeds()
```

## ADD PARAMETERS TO GET REQUESTS

A GET response can contain parameters in the URL, like this: `https://site.com/?foo=bar`.

With Axios you can perform this by simply using that URL:

```
axios.get('https://site.com/?foo=bar')
```

or you can use a `params` property in the options:

```
axios.get('https://site.com/', {
  params: {
    foo: 'bar'
  }
})
```

## POST REQUESTS

Performing a POST request is just like doing a GET request, but instead of `axios.get`, you use `axios.post`:

```
axios.post('https://site.com/')
```

An object containing the POST parameters is the second argument:

```
axios.post('https://site.com/', {
  foo: 'bar'
})
```









 





