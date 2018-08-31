1.Definition

  Simply put, hoisting is declarations being moved to the top of the code(JS scope) by the JS interpreter

2.What happens

  upon an event of hoisting, the function and variable declarations are added to memory during the compile phase

3. function/variable declarations v. function/variable expressions

- function declaration must begin with "function" ; variable declarations must start with "var"
- variable expressions : in var x = 4,  var x is variable declaration ; x = 4 is variable expression

​    types of function expressions

```////
//anonymous function expression
var a = function(){
    return 3;
}

//named function expression
var a = function bar(){
    return 3; 
}

//self invoking function expression
(function sayHello(){
    alert('hello!');
})();
```

4.examples

```
1.
var x = 12;
(function(){
    console.log(x);
    var x = 13;
    console.log(x);
})();
//undefined <-- first console.log(x)
//13. <--second console.log(x)

2-1.two sets of codes below are equivalent 
logSomething();
var logSomething = function logSomething(){
    console.log(11);
}
//uncaught TypeError : logSomething is not a function

2-2. 
var logSomething;
logSomething();
logSomething = function logSomething(){
    console.log(11);
}
//uncaught TypeError: logSomething is not a function
//why? only the logSomething variable is hoisted to the top of the JS scope, NOT the function expression( logSomething = function logSomething(){...} )
```

5.quizzes

```
1.
function foo(){
    function bar(){
        return 3;
    }
    return bar();
    function bar(){
        return 8;
    }
}
console.log(foo());

2. 
function foo(){
    var bar = function(){
        return 3;
    };
    return bar();
    var bar = function(){
        return 8;
    };
}
console.log(foo());

3. 
console.log(foo());
function foo(){
    var bar = function(){
        return 3;
    };
    return bar();
    var bar = function(){
        return 8;
    };
}

4. 
function foo(){
    return bar();
    var bar = function(){
        return 3;
    };
    var bar = function(){
        return 8;
    };
}
console.log(foo());
```

Answers:

```
1.
output:8
function declarations are used, hence they get hoisted as shown below,
function foo(){
   //define bar once
    function bar(){
        return 3;
    }
    //redefine it
    function bar(){
        return 8;
    }
    //return its invocation
    return bar(); 
}
console.log(foo());   //8

2.variable declarations are hoisted
output: 3
function foo(){
   //each function variable declaration of the two function expressions
    var bar = undefined; 
    var bar = undefined;
    //first function expression is executed
    bar = function(){
        return 3;
    };
    //function created by first function expression
    return bar();
    //second function expression is unreachable 
}
console.log(foo());    //3 

3.the foo() function gets hoisted
output: 3
//the foo() function is hoisted
function foo(){
    var bar = undefined;
    var bar = undefined;
    bar = function(){
        return 3;
    };
    return bar();
    //like in quiz #2, second function is unreachable
}
console.log(foo());  //3

4.
output: type error
function foo(){
    var bar = undefined;
    var bar = undefined;
    return bar(); //TypeError: "bar not defined"
    //neither function expression is reached
}
//Type Error: bar is not a function
```



