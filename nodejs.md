```
http.createServer(function(req, res){
  res.writeHead
  res.write(' ... ');
  res.end();
})

```

The requestListener is a function that is called each time the server gets a request.

The requestListener function is passed as a parameter to the http.createServer() method.

The requestListener function handles *requests* from the user, and also the *response* back to the user

Syn: 

http.createServer(requestListener);

requestListener : function(req, res)

The http.createServer() method turns your computer into an HTTP server.

The http.createServer() method creates an [HTTP Server object](https://www.w3schools.com/nodejs/obj_http_server.asp).

The [HTTP Server object](https://www.w3schools.com/nodejs/obj_http_server.asp) can listen to ports on your computer and execute a function, a [requestListener](https://www.w3schools.com/nodejs/func_http_requestlistener.asp), each time a request is made.



Node.js has a built-in module called HTTP, which allows Node.js to transfer data over the Hyper Text Transfer Protocol (HTTP).

To include the HTTP module, use the `require()` method:

var http = require('http');

HTTP Header :

```
 res.writeHead(200, {'Content-Type': 'text/html'});
```

The first argument of the `res.writeHead()` method is the status code, 200 means that all is OK, the second argument is an object containing the response headers.



Keyword require()

- used to import modules
- how to define it

```
var require = function(path){
    //...

return module.exports;
};
```

Require greeting.js in main.js

```
//main.js
var greetings = require("./greetings.js");

//the above is equivalent to :
var greetings = {
    sayHelloInEnglish: function(){
        return "HELLO";
    },
    
    sayHelloInSpanish: function(){
        return "Hola";
    }
};

//now, can access to the publicaly available methods of greetings.js as a property //of the greetings variable in main.js

//main.js
var greetings = requre("./greetings.js");

//"Hello"
greetings.sayHelloInEnglish();

//"Hola"
greetings.sayHelloInSpanish();
```

```
var http = require('http');

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.write('Hello World!');
    res.end();
}).listen(8080);
```

Node.js server.listen() Method

- creates a listener on the specified port or path
- server.listen(*port*, *hostname*, *backlog*, *callback*);

HTTP Request Methods

- GET
- POST

Request Body

When receiving a `POST` or `PUT` request, the request body might be important to your application. Getting at the body data is a little more involved than accessing request headers. The `request` object that's passed in to a handler implements the [`ReadableStream`](https://nodejs.org/api/stream.html#stream_class_stream_readable) interface. This stream can be listened to or piped elsewhere just like any other stream. We can grab the data right out of the stream by listening to the stream's `'data'` and `'end'` events.

The chunk emitted in each `'data'` event is a [`Buffer`](https://nodejs.org/api/buffer.html). If you know it's going to be string data, the best thing to do is collect the data in an array, then at the `'end'`, concatenate and stringify it.

```
let body = [];
request.on('data', (chunk) => {
  body.push(chunk);
}).on('end', () => {
  body = Buffer.concat(body).toString();
  // at this point, `body` has the entire request body stored in it as a string
});
```



File System 

1.

fs.readFile(filename, [encoding], [callback])

```
fs.readFile('/etc/passwd', function(err, data){
  if(err) throw err;
  console.log(data);
});
```

-Asynchronously read the entire contents of a file

-the callback is passed two arguments(err, data), where data is the contents of the file

-if no encoding is specified, then the raw buffer is returned

Why does the code below returns undefined?

```
var content;
fs.readFile('./Index.html', function read(err, data) {
    if (err) {
        throw err;
    }
    content = data;
});
console.log(content);
```

- the function defined here is an asynchronous callback
- it executes when the file loading has completed
- when readFile is called, control is returned immediately and the next line(conte=data) is executed
- Hence, when you call console.log, a callback hasn't yet been invoked and the content hasn't yet been set

solution: wrap async calls in function

```
function readContent(callback) {
    fs.readFile("./Index.html", function (err, content) {
        if (err) return callback(err)
        callback(null, content)
    })
}

readContent(function (err, content) {
    console.log(content)
})
```

There are four fundamental stream types within Node.js:

- [`Writable`](https://nodejs.org/api/stream.html#stream_class_stream_writable) - streams to which data can be written (for example, [`fs.createWriteStream()`](https://nodejs.org/api/fs.html#fs_fs_createwritestream_path_options)).
- [`Readable`](https://nodejs.org/api/stream.html#stream_class_stream_readable) - streams from which data can be read (for example, [`fs.createReadStream()`](https://nodejs.org/api/fs.html#fs_fs_createreadstream_path_options)).
- [`Duplex`](https://nodejs.org/api/stream.html#stream_class_stream_duplex) - streams that are both `Readable` and `Writable` (for example, [`net.Socket`](https://nodejs.org/api/net.html#net_class_net_socket)).
- [`Transform`](https://nodejs.org/api/stream.html#stream_class_stream_transform) - `Duplex` streams that can modify or transform the data as it is written and read (for example, [`zlib.createDeflate()`](https://nodejs.org/api/zlib.html#zlib_zlib_createdeflate_options)).

Additionally this module includes the utility functions [pipeline](https://nodejs.org/api/stream.html#stream_stream_pipeline_streams_callback) and [finished](https://nodejs.org/api/stream.html#stream_stream_finished_stream_callback).



**[request(url).pipe()]**

##### Event: 'pipe'[#](https://nodejs.org/api/stream.html#stream_event_pipe)

Added in: v0.9.4

- `src` [](https://nodejs.org/api/stream.html#stream_class_stream_readable) source stream that is piping to this writable

The `'pipe'` event is emitted when the [`stream.pipe()`](https://nodejs.org/api/stream.html#stream_readable_pipe_destination_options) method is called on a readable stream, adding this writable to its set of destinations.

```
const writer = getWritableStreamSomehow();
const reader = getReadableStreamSomehow();
writer.on('pipe', (src) => {
  console.error('something is piping into the writer');
  assert.equal(src, reader);
});
reader.pipe(writer);
```

```
const fs = require('fs')
const http = require('http')

const request = require('request')

let url = 'https://upload.wikimedia.org/wikipedia/en/8/8a/Text_placeholder_image.jpg' // some jpg

request(url).pipe(fs.createWriteStream('1.jpg'))

http.createServer((req, res) => {
  fs.readFile('1.jpg', (error, content) => {
    if (error) {
      // handle error
      return
    }

    res.writeHead(200, 'image/jpeg')
    res.end(content, 'utf-8')
  })
}).listen(3000)
```





















 