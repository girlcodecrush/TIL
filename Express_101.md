var requestWithSession = request.defaults({jar:true});

What's jar?

`request` provides the `jar()` and `cookie()` methods

Cookies are small pieces of data that are passed back and forth between the client and server with every HTTP transaction

Individual cookies are created using the `cookie()` method. On line 3 of the following example, a cookie is created which specifies the user’s name. The cookie is then added to the cookie jar created on line 2. When the HTTP request is made, the `jar` parameter is used to send the cookie jar to the server.

```
var request = require("request");
var jar = request.jar();
var cookie = request.cookie("name=John");

jar.add(cookie);
request({
  uri: "http://www.cjihrig.com/development/php/hello_cookies.php",
  method: "GET",
  jar: jar
}, function(error, response, body) {
  console.log(body);
});
```

**What does var app = express() mean?** 

The real difference between `require('express')` and `express()` is that `require('express')` allows you to have access to any public functions or properties exposed by [`module.exports`](https://nodejs.org/api/modules.html#modules_module_exports).

The `express()` syntax is the equivalent of saying `new express()`. It creates a new instance of `express` that you can then assign to a variable and interact with.

That is why the standard creation pattern for Express is

```
// Import the Express module
var express = require('express');

// Create a new Express Instance
var app = express();
```

API

```
var express      = require('express')
var cookieParser = require('cookie-parser')
 
var app = express()
app.use(cookieParser())
```

```
var express      = require('express')
var cookieParser = require('cookie-parser')
 
var app = express()
app.use(cookieParser())
 
app.get('/', function(req, res) {
  console.log('Cookies: ', req.cookies)
})
 
app.listen(8080)
 
// curl command that sends an HTTP request with two cookies
// curl http://127.0.0.1:8080 --cookie "Cho=Kim;Greet=Hello"
```

### req.session

To store or access session data, simply use the request property `req.session`, which is (generally) serialized as JSON by the store

#### Session.regenerate(callback)

To regenerate the session simply invoke the method. Once complete, a new SID and `Session`instance will be initialized at `req.session` and the `callback` will be invoked.

```
req.session.regenerate(function(err) {
  // will have a new session here
})
```

### res.redirect([status,] path)

Redirects to the URL derived from the specified `path`, with specified `status`, a positive integer that corresponds to an [HTTP status code](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) . If not specified, `status` defaults to “302 “Found”.

```
res.redirect('/foo/bar');
res.redirect('http://example.com');
res.redirect(301, 'http://example.com');
res.redirect('../login');
```



[`reset`](http://backbonejs.org/#Collection-reset) sets the collection with an array of models that you specify:

```
collection.reset( [ { name: "model1" }, { name: "model2" } ] );
```

[`fetch`](http://backbonejs.org/#Collection-fetch) retrieves the collection data from the server, using the URL you've specified for the collection.

```
collection.fetch( { url: someUrl, success: function(collection) {
    // collection has values from someUrl
} } );
```

**fetch**`model.fetch([options])` 
Merges the model's state with attributes fetched from the server by delegating to [Backbone.sync](http://backbonejs.org/#Sync). Returns a [jqXHR](http://api.jquery.com/jQuery.ajax/#jqXHR). Useful if the model has never been populated with data, or if you'd like to ensure that you have the latest server state. Triggers a `"change"` event if the server's state differs from the current attributes. `fetch` accepts`success` and `error` callbacks in the options hash, which are both passed `(model, response, options)` as arguments.

```
// Poll every 10 seconds to keep the channel model up-to-date.
setInterval(function() {
  channel.fetch();
}, 10000);
```

BOOKSHELF.js

### Connecting to a Database

All of the lower-level functions, like connecting to the database, are handled by the underlying Knex library. So, naturally, in order to initialize your `bookshelf` instance you'll need to create a `knex` instance first, like this:

```
var knex = require('knex')({  
    client: 'sqlite3',
    connection: {
        filename: './db.sqlite'
    }
});

var bookshelf = require('bookshelf')(knex);  
```

### Connecting to a Database

All of the lower-level functions, like connecting to the database, are handled by the underlying Knex library. So, naturally, in order to initialize your `bookshelf` instance you'll need to create a `knex` instance first, like this:

```
var knex = require('knex')({  
    client: 'sqlite3',
    connection: {
        filename: './db.sqlite'
    }
});

var bookshelf = require('bookshelf')(knex);  
```

And now you're able to use the `bookshelf` instance to create your models.

### Setting up the Tables

Knex, as its own website states, is a "batteries included" SQL query builder, so you can do just about anything through Knex that you'd want to do with raw SQL statements. One of these important features is table creation and manipulation. Knex can be used directly to set up your schema within the database (think database initialization, schema migration, etc).

you'll want to create your table using `knex.schema.createTable()`, which will create and return a table object that contains a bunch of [schema building](http://knexjs.org/#Schema-Building) functions, like `table.increments()`, `table.string()`, and `table.date()`. For each model you create, you'll need to do something like this for each one:

```
knex.schema.createTable('users', function(table) {   
table.increments(); 
table.string('name'); 
table.string('email', 128); 
table.string('role').defaultTo('admin'); 
table.string('password'); table.timestamps(); 
});
```



how to implement authentication in [Express.js]

















