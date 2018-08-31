1.call & apply invocation  :  this refences to the object method 

​    this  == first argument 

`<p id="demo"></p>`

`<script>`
`var person = {`
    `fullName: function() {`
        `return this.firstName + " " + this.lastName;`
    `}`
`}`
`var person1 = {`
    `firstName:"John",`
    `lastName: "Doe",`
`}`
`var x = person.fullName.apply(person1);` 
`document.getElementById("demo").innerHTML = x;` 
`</script>`

Output : John Doe



2. using apply to append an array to another 

     - use `push` to append an element to an array
- to append to our existing array with the original array unchanged -> after compling, the original array is still same 
- concat : not append to the existing array, but creates and returns a new array after compling 

```
var array = ['a', 'b'];
var elements = [0, 1, 2];
array.push.apply(array, elements);
console.info(array); // ["a", "b", 0, 1, 2]
```

var array = ['a','b', 0,1,2]   



3. Call & apply explained

- this references to the current object, the calling object

- the method is inherited in another object

- can use arguments for the argumentsArray  : arguments is a local variable 

  

4. Using .call() to convert array-like arguments to a true array 

​         function getMax () {         

// convert argumens to a true array   

var argsArr = Array.prototype.slice.call(arguments) ; 

//use .reduce() method to find the maximum number 

  var maxVal = argsArr.reduce(function(accumulator,current){
      if(accumulator > current){ 
          return accumulator; 
      } else {
        return current;
      }
  });  
     return maxVal; 
}
console.log(getMax(4,5,2,7,3)); 



5. object.method() & "this"

    - functions stored in object properties are "methods"
    - methods allow objects to act like object.doSomething()
    - Methods can reference the object as this
    - the value of this is defined at run-time
    - When a function is declared, the function may use this, but this has no value until the funcion is called
    - when a function is called in the method syntax: object.method(), the value of this during the call is object

```
var user = {
    name: "John",
    age: 30,
    sayHi(){                //same as sayHi : function(){ 
        alert(this.name);
    }
};
user.sayHi();  // John
```

```
var user = { name: "John" };
var admin = { name: "Admin" };

function sayHi(){
    alert(this.name);
}

user.f = sayHi;
admin.f = sayHi;
  
user.f() ;   // John      (this == user)
admin.f() ;  //Admin      (this == admin)

user['f']();    //John     (*dot or square brackets access the method)
admin['f']();   //Admin
```

```
function sayHi(){
    alert(this);
}
sayHi();       //undefined [object Window]
```

Q. What is the result of this code?

```
let user = {
  name: "John",
  go: function() { alert(this.name) }
}

(user.go)()
```

Solution:   Error! 

   Why: a semicolon is missing after user = { .....}



**<u>bind</u>** : 

- use to *fix "this" in an object method* 
- *pass it somewhere*, such as setTimeout

 for(var key in object) { 

​    if(typeof object[key]==="function"){

​        object[key] = object[key].bind(object)

​       }

   }

-when using setTimeout with object methods or passing object methods along, there is a known problem: "losing `this`"

-once an object method is passed somewhere separately from the object, this is lost

```
1.
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  }
};

setTimeout(user.sayHi, 1000); // Hello, undefined!

2.
var user = {
    firstName: 'John',
    sayHi(){
        alert('Hello, ' + this.firstName + "!");
    }
};
var sayHi = user.sayHi; 

setTimeout(sayHi, 2000);  // Hello, undefined!

```

-the output shows not "John" as this.firstName, but undefined --> ***because setTimeout got the function user.sayHi, separately from the object***

-for this.firstName, it tries to get window.firstName, which doesn't exist 

solution 1:

A wrapping function: 

```
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  }
};

setTimeout(function() {
  user.sayHi(); // Hello, John!
}, 1000);
```

Works! Because it receives user from the outer lexical scope

HOWEVER, still vulnerable...

-before setTimeout triggers, the object user changes its value and suddenly will call the wrong object 

==> here bind kicks in 

```
let boundFunc = func.bind(context);
```

func.bind(context) passes the call to func setting this=context

calling boundFunc is like func with fixed this

in the example below, funcUser passes a call to func with this=user:

```
let user = {
  firstName: "John"
};

function func() {
  alert(this.firstName);
}

let funcUser = func.bind(user);
funcUser(); // John
```

Solution 2]  w/ 'bind' *

```
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  }
};

let sayHi = user.sayHi.bind(user); // (*)

sayHi(); // Hello, John!

setTimeout(sayHi, 1000); // Hello, John!
```

- take the method user.sayHi and bind it to user

- sayHi is a "bound" function that can be called alone or passed to setTimeout

  

In the following ex, arguments are passed and only this is fixed by bind:

```
let user = {
  firstName: "John",
  say(arg) {
    alert(arg + ", " + this.firstName);
  }
};

let say = user.say.bind(user);

say("Hey"); // Hey, John ("Hey" argument is passed to say)
say("Bye"); // Bye, John ("Bye" is passed to say)
```











