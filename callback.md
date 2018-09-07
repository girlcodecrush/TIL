A callback function 

passed to another function(aka, "otherFunction") as a parameter

Executed(called) inside the otherFunction

in jQuery: 

```
//Note that the item in the click method's parameter is a function, not a variable.
//The item is a callback function
$("#btn_1").click(function() {
  alert("Btn 1 Clicked");
});
```

Explained: a function is passed as a parameter to the on-click event ; the click method will call/execute the callback function we passed to it

in JS; 

```
var friends = ["Mike", "Stacy", "Andy", "Rick"];

friends.forEach(function (eachName, index){
console.log(index + 1 + ". " + eachName); // 1. Mike, 2. Stacy, 3. Andy, 4. Rick
});
```

Explained: a function passed to the forEach method as a parameter



more to remember:

when we pass a callback function as a argument to another function, we are only passing the fucntion definition, NOT executing the function in the parameter --> not passing the function with a pair of executing parenthesis() like we do when executing a function ; thus, the callback function isn't executed immediately

callback is a closure : in a sense that the callback function is executed at some point inside th contatining function's body. -> can acceess the containing function's variables and the variables from the global scope

Passing parameters to callback functions:

can pass any of the containing function's properties(or global properties) as parameters to the callback function; in the following example, 'options' is passed as a parameter to the callback function

```
 // global variable
var allUserData = [];

// generic logStuff function that prints to console
function logStuff (userData) {
    if ( typeof userData === "string")
    {
        console.log(userData);
    }
    else if ( typeof userData === "object")
    {
        for (var item in userData) {
            console.log(item + ": " + userData[item]);
        }

    }

}

// A function that takes two parameters, the last one a callback function
function getInput (options, callback) {
    allUserData.push (options);
    callback (options);

}
```

Also, can pass a global variable and a local variable :

```
//Global variable
var generalLastName = "Clinton";

function getInput (options, callback) {
    allUserData.push (options);
// Pass the global variable generalLastName to the callback function
    callback (generalLastName, options);
}
```

check if the callback is a REAL function:  (*important! : otherwise, our code will result in a runtime error)

```
function getInput(options, callback) {
    allUserData.push(options);

    // Make sure the callback is a function
    if (typeof callback === "function") {
    // Call it, since we have confirmed it is callable
        callback(options);
    }
}
```