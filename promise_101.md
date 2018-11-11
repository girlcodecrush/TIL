```
function loadScript(src) {
    let script = document.createElment('script');
    script.src = src;
    document.head.append(script);
}

function loadScript(src, callback) {
    let script = document.createElement('script');
    script.src = src;
    
    script.onload = () =>callback(script);
    
    document.head.append(script);
}

// if wanna call new functions from the script, should write that in the callback

loadScript('/my/script.js', function(){
   //the callback runs after the script is loaded
   newFunction();   //this WORKS!!
   ...
});

loadScript('/my/script/js', function(error, script){//-> "error-first callback"style
   if(error){
       //handle error
   } else {
       //script loads successfully
   }
});

//1. the first arg of CB is reserver for an error if occurs, then callback(err) is called
2. the second arg/args are for the successful results, then cb(null, result1, result2,..) is called
```

- a promise is a JS object that links the producing code and the consuming code together
- producing code takes whatever time it needs to produce the promised result
- the promise make the result available to all of the subscribed code when ready

Constructor Syntax for a promise obj:

```
let promise = new Promise(function(resolve, reject){
   //executor (the producing code) 
});
```

- the function passed to new Promise(constructor) is an executor
  - the promise constructor takes one argument, a callback with two parameters, resolve and reject
- the resulting promise object's properties: 
  -  State - initially pending then changes to either 'fulfilled' or 'rejected'
  - Result - an arbitrary value of your choosing, initally undefined
- when the executor finishes the job, calls one of the functions that it gets as args
  - resolve(value) - to indicate that the job finished successfully
    - Sets state to 'fulfilled'
    - sets result to value
  - reject(error) - to indicate that an error occurred
    - sets state to 'rejected'
    - sets result to error

```
let promise = new Promise(function(resolve, reject){
   //the function is executed automatically when the promise is constructed
   
   setTimeout(() => resolve('done!), 1000);
});

let promise = new Promise(function(resolve, reject){
   setTimeout(() => reject(new Error('Whoops!')), 1000);  
});


promise.then(function(result){
  console.log(result);  //worked!
}, function(err){
  console.log(err);  // Error: "It broke" 
});
```

- the executor is called automatically and immediately by the new Promise
- the executor receives two args : resolve and reject
  - these functions are pre-defined by the JS enegine -> no need to create them , instead write the executor to call them when ready
- then() takes two arguments 
  - a callback for a success case and another for the failure case 

Promisifying XMLHttpRequest:

a function to make a GET request

```
function get(url) {
  //return a new promise
  return new Promise(function(resolve, reject){
    var req = new XMLHttpRequest();
    req.open('GET', url);
    
    req.onload = function(){
      //This is called even on 404, etc
      // so check the status
      if(req.status == 200){
        //resolve the proise with the response text
        resolve(req.response);
      }else {
        //otherwise reject with the status text
        //which will hopefully be a meaningful error
        reject(Error(req.statusText));
      }
    };
    //handle netork errors
    req.onerror = function(){
      reject(Error('Network Error'));
    };
    //make the request
    req.send();
  });
}


get('story.json').then(function(response){
  console.log('Success!', response);
}, function(error){
  console.log('Failed!', error);
})
```

**Chaining**

transforming values: 

transform values simply by returning the new value

```
var promise = new Promise(function(resolve, reject){
  resolve(1);
});
promise.then(function(val){
  console.log(val);  // 1
  return val + 2; 
}).then(function(val){
   console.log(val)  // 3
})

//* The response is JSON 
get('story.json').then(function(response){
  return JSON.parse(response);
}).then(function(response){
  console.log('Yey JSON!', response);
})

//Since JSON.parse() takes a single arg and returns a transformed value, can make a shortcut;
get('story.json').then(JSON.parse).then(function(response){
  console.log('Yey JSON!', response);
})

//using getJSON() function
function getJSON(url){
  return get(url).then(JSON.parse);
}

//getJSON() still returns a promise, one that fetches a url then parses the response as JSON
```

**Queuing asynchronous actions**

: chain then() callbacks to run async actions in sequence

- if you return a value, the next then() is called with that value
- if you return sth promise-like, the next then() waits on it
- then() is ONLY called when that promise settles(succeeds/fails)

```
getJSON('story.json').then(function(story){
  return getJSON(story.chapterUrls[0]);
}).then(function(chapter1){
   console.log('Got chapter 1!', chapter1);
})

//here,we make an async request to story.json, which gives us a set of urls to request, then we request the first of those
```

**Error Handling**

```
get('story.json').then(function(respons){
  console.log('Success!', response);
}, function(error){
  console.log('Failed!', error);
})

//instead of the above, can use catch()
get('story.json').then(function(respons){
  console.log('Success!', response);
}.catch(function(error){      
  console.log('Failed!', error);
})
//catch() is only sugar for then(undefined, func), the above two are not idential
//the latter is equivalent to:
get('story.json').then(function(respons){
  console.log('Success!', response);
}.then(undefined, function(error){      
  console.log('Failed!', error);
})
```











Async/ Await

Async functions

```
async function f(){
    return 1;
}
```

- Async before function means: a function always returns a promise

- if the code has return <non-promise> in it, then JS automatically wraps it into a resolved promise with that value

  

```
async function f(){
    return 1;
}
f().then(alert);  //1

the above is the same as the below
async function f(){
    return Promise.resolve(1);
}
f().then(alert);  //1
```

Await

- Works only inside async functions
- makes JS wait until that promise settles and returns its result

```
let value = await promise;

another ex;

async function f(){
    let promise = new Promise((resolve, reject) => {
       setTimeout() => resolve('done!), 1000) 
    });
    
    let result = await promise;  // wait till the promise resolves (*)
    
    alert(result); // 'done!'
}

f();
```

- the function execution pauses at the line(*) and resumes when the promise settle
- can't use await in regular functions : if try to use await in non-async function, would be a syntax error

```
function f() {
    let promise = Promise.resolve(1);
    let result = await promise;  //syntax error
}
```

- IMPORTANT! - await ONLY works inside async function



Promise Chaining





