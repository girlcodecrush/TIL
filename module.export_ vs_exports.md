The **module.exports** or **exports** is a special object which is included in every JS file in the Node.js application by default. *module* is a variable that represents current module and *exports* is an object that will be exposed as a module. So, whatever you assign to *module.exports* or *exports*, will be exposed as a module.

Let's see how to expose different types as a module using module.exports.

## Export Literals

As mentioned above, *exports* is an object. So it exposes whatever you assigned to it as a module. For example, if you assign a string literal then it will expose that string literal as a module.

The following example exposes simple string message as a module in Message.js.

Message.js

```
module.exports = 'Hello world';

//or

exports = 'Hello world';
```

Now, import this message module and use it as shown below.

app.js

```
var msg = require('./Messages.js');

console.log(msg);
```

Run the above example and see the result as shown below.

C:\> node app.js 
Hello World

Note: You must specify './' as a path of root folder to import a local module. However, you do not need to specify path to import Node.js core module or NPM module in the require() function.

## Export Object

*exports* is an object. So, you can attach properties or methods to it. The following example exposes an object with a string property in Message.js file.

Message.js

```
exports.SimpleMessage = 'Hello world';

//or

module.exports.SimpleMessage = 'Hello world';
```

In the above example, we have attached a property "SimpleMessage" to the exports object. Now, import and use this module as shown below.

app.js

```
var msg = require('./Messages.js');

console.log(msg.SimpleMessage);
```

In the above example, require() function will return an object `{ SimpleMessage : 'Hello World'}`and assign it to the msg variable. So, now you can use msg.SimpleMessage.

Run the above example by writing `node app.js` in the command prompt and see the output as shown below.

C:\> node app.js 
Hello World

The same way as above, you can expose an object with function. The following example exposes an object with log function as a module.

Log.js

```
module.exports.log = function (msg) { 
    console.log(msg);
};
```

The above module will expose an object- `{ log : function(msg){ console.log(msg); } } `. Use the above module as shown below.

app.js

```
var msg = require('./Log.js');

msg.log('Hello World');
```

Run and see the output in command prompt as shown below.

C:\> node app.js 
Hello World

You can also attach an object to module.exports as shown below.

data.js

```
module.exports = {
    firstName: 'James',
    lastName: 'Bond'
}
```

app.js

```
var person = require('./data.js');
console.log(person.firstName + ' ' + person.lastName);
```

Run the above example and see the result as shown below.

C:\> node app.js 
James Bond

## Export Function

You can attach an anonymous function to exports object as shown below.

Log.js

```
module.exports = function (msg) { 
    console.log(msg);
};
```

Now, you can use the above module as below.

app.js

```
var msg = require('./Log.js');

msg('Hello World');
```

The msg variable becomes function expression in the above example. So, you can invoke the function using parenthesis (). Run the above example and see the output as shown below.

C:\> node app.js 
Hello World

## Export function as a class

In the JavaScript, a function can be treated like a class. The following example exposes a function which can be used like a class.

Person.js

```
module.exports = function (firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.fullName = function () { 
        return this.firstName + ' ' + this.lastName;
    }
}
```

The above module can be used as shown below.

app.js

```
var person = require('./Person.js');

var person1 = new person('James', 'Bond');

console.log(person1.fullName());
```

As you can see, we have created a person object using new keyword. Run the above example as below.

C:\> node app.js 
James Bond

In this way, you can export and import a local module created in a separate file under root folder.

Node.js also allows you to create modules in sub folders. Let's see how to load module from sub folders.

## Load Module from Separate Folder

Use the full path of a module file where you have exported it using module.exports. For example, if log module in the log.js is stored under "utility" folder under the root folder of your application then import it as shown below.

app.js

```
var log = require('./utility/log.js');
```

In the above example, **.** is for root folder and then specify exact path of your module file. Node.js also allows us to specify the path to the folder without specifying file name. For example, you can specify only utility folder without specifing log.js as shown below.

app.js

```
var log = require('./utility');
```

In the above example, Node will search for a package definition file called package.json inside utility folder. This is because Node assumes that this folder is a package and will try to look for a package definition. The package.json file should be in a module directory. The package.json under utility folder specifies the file name using "main" key as below.

./utility/package.json

```
{
    "name" : "log",
    "main" : "./log.js"
}
```

Now, Node.js will find log.js file using *main* entry in package.json and import it.