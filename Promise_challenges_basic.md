

1.Callback Review : Implement these functions following the node stsyle callback pattern

```
var fs = require('fs');
var request = require('request');

//This function should retrieve the first line of the file at 'filePath'
var pluckFirstLineFromFile = function (filePath, callback) {
  // TODO
  //fs.readFile(path, encoding, callback)
  fs.readFile(filePath, 'utf8', function(err, data){
     if(err){
        callback(err)
     } else {
         callback(null, data.split('\n')[0])
     }
  });
};

//This function should retrieve the status code of a GET request to 'url'
var getStatusCode = function(url, callback){
  //TODO
  request(url, 'utf8', function(err, response){
    if(err){
      callback(err)
    }else{
     callback(null, response.statusCode)
    }
  }); 
};

//Export these functions so we can test them and reuse them in the following challenges.
module.exports = {
  getStatusCode: getStatusCode,
  pluckFirstLineFromFile: pluckFirstLineFromFile
};
```

2.Promise Constructor : 

Implement these "***promise-returning***" functions. Any successful value should be available in the next 'then' block chained to the function invocation while errors should be available in the 'catch' block.

```
var fs = require('fs');
var request = require('request');

//This function should retrieve the first line of the file at 'filePath'
var pluckFirstLineFromFileAsync = function(filePath){
  //TODO
  //Create a new promise; Promise is a promise constructor ; function(res, rej) is // an executor
  return new Promise (function(resolve, reject){
    fs.readFile(filePath, 'utf8', function(err, data){
      if(err){
          reject(err);
      } else {
        resolve(data.split('\n')[0])
      }
    }); 
  });
};

//This function should retrieve the status code of a GET request to 'url'
var getStatusCodeAsync = function(url){
  //TODO
  return new Promise(function(resolve, reject){
    request(url, 'utf8', function(err, response){
      if(err){
        reject(err);
      } else {
        resolve(response.statusCode)
      }
    });
  });
};

//Export these functions so we can test them and reuse them in later challenges
module.exports = {
  getStatusCodeAsync: getStatusCodeAsync, 
  pluckFirstLineFromFileAsync: pluckFirstLineFromFileAsync
}; 
```

3.promisification

Create the promise returning 'Async' suffixed version of the functions. Promisify them if possible, otherwise roll your own promise returning function.

```
var fs = require('fs');
var request = require('request');
var crypto = require('crypto');
var util = require('util');

//A. Asynchronous http request
var getGitHubProfile = function(user, callback){
  var options = {
    url: 'https://api.github.com/users/' + user,
    headers: {'User-Agent': 'request'},
    json: true
  };
  
  request.get(options, function(err, res, body){
    if(err){
      callback(err,null)  
    }
    else if(body.message){
      callback(new Error('Failed to get GitHub profile: ' + body.message), null);
    }else{
     callback(null, body);   
    }
  });
};

//TODO
var getGitHubProfileAsync = util.promisify(getGitHubProfile);

//B. Asynchronous token generation
var generateRandomToken = function(callback){
  crypto.randomBytes(20, function(err, buffer){
    if(err) {
      return callback(err, null);
    } 
    callback(null, buffer.toString('hex'));
  });  
};
//TODO
var generateRandomTokenAsync = util.promisify(generateRandomToken);

//C. Asynchronous file manipulation
var readFileAndMakeItFunny = function(filePath, callback){
  fs.readFile(filePath, 'utf8', function(err, file){
    if(err){
      return callback(err);
    } 
    
    var funnyFile = file.split('\n')
      .map(function(line){
        return line + ' lol'; 
      })
      .join('\n');
    
    callback(null, funnyFile);
    
  });  
};

//TODO
var readFileAndMakeItFunnyAsync = util.promisify(readFileAndMakeItFunny);

//Export these functions so we can test them and reuse them later
module.export = {
  getGitHubProfileAsync:getGitHubProfileAsync,
  generateRandomTokenAsync: generateRandomTokenAsync, 
  readFileAndMakeItFunnyAsync: readFileAndMakeItFunnyAsync
};
```

**Promisify**

a. Create the promisify function that takes one argument, function and retuns a function. When the returned function is called, it will return a promise 

* the keyword function below is used because will need an access to this, which is not available to arrow functions
* the original function, func, can take a callback using apply - func.apply(this, ...args, callback)

```
function promisify(func){
  return () =>
    new Promise((resolve, reject) => {})
}
```

b. putting things together, it looks like this

```
function promisify(func){
  return (...args) => 
    new Promise((resolve, reject) => {
      const callback = ??? 
      
      func.apply(this, [..args, callback]);
    })
}
```

c. create the callback; the callback calls either resolve or reject based on the presence of err

```
function promisify(func){
  return (...args) =>
    new Promise((resolve, reject)=> {
      const callback = (err, data) => err ? reject(err) : resolve(data)
      
      func.apply(this, [...args, callback])
    })
}
```

4.Chaining

Write a function without callbacks that:

(1) reads a GitHub username from a 'readFile Path' (the username will be the first line of the file)

(2) then, sends a request to the GitHub API for the user's profile

(3) then, writes the JSON response of the API to 'writeFilePath'

```
var fs = require('fs');
var util = require('util');

var pluckFirstLineFromFileAsync = require('./promiseConstructor).pluckFirstLineFromFileAsync;
var getGitHubProfileAsync = require('./promisification).getGitHubProfileAsync;
var writeFileAsync = util.promisify(fs.writeFile);

var fetchProfilAndWriteToFile = function(readFilePath, writeFilePath){
  //TODO
  return pluckFirstLineFromFileAsync(readFilePath)
  .then(getGitHubProfileAsync)
  .then(function(profile){
   return writeFileAsync(writeFilePath, JSON.stringify(profile));   
  });
};

//Export these functions for testing
module.export = {
  fetchProfileAndWriteToFile : fetchProfileAndWriteToFile
};


//the below is the same as the above
return pluckFirstLineFromFileAsync(readFilePath)
  .then(function(username){
    return getGitHubProfileAsync(username);  
  });
  .then(function(userProfile){
    return writeFileAsync(writeFilePath, JSON.stringify(profile));  
  });
  .catch(function(err){
    console.log(err);
  });
```























