Closure: 

- 내부함수가 외부함수의 지역변수에 접근할 수 있고, 외부함수는 자신의 지역변수를 사용하는 내부함수가 소멸될 때까지 소멸되지 않고 자신의 상태를 유지하여 내부함수에 의해 소멸하게 되는 특성
- execution context(실행 컨텍스트) 관점에서 보면, 내부함수가 유효한 상태에서 외부함수가 먼저 종료된 후 자신의execution context가 반환되어도, 그 실행 컨텍스트가 가지고 있는 변수나 함수선언 등의 정보(Activation Object)를 내부함수가 scope chain을 통해 참조할 수 있도록 하는 것 
- 즉, 외부함수가 이미 반환되었어도 외부함수 내의 변수를 내부함수가 필요로하는 한 그 변수는 계속 유지된다는 것 --> 내부함수가 외부함수의 변수에 접근 가능 

Ex. 1: the relationship btw inner function and outer function from a closure standpoint

```
function outer (){
  var title = 'coding is fun' ; 
  return function(){
        alert(title);
    }
}
var inner = outer() ;
inner(); 
```

take closer look at how a closure works:

1. outer() returns the inner function( ){ alert(title); }

2. after outer() executes, the result goes to the variable, inner 

3. when the function that is included in the variable, inner, (that's, () in inner(); ) is called, 'coding is fun' shows up 

4. here, closure kicks in !  -->  when outer() returns another function, which is the inner function( function(){ alert(title);} ) , outer() get out of its life cycle(the function's exeuction power is gone). 

5. nonetheless, when we call the function embedded in the variable, inner, although outer() doens't have an execution power, the inner function still can access to the local variable of the outer function(outer() ). 

6. the local variable of the outer function is still available for use by the inner function which is derived from the outer function 

7. In a nutshell, the inner function can access to the outer function and even after the outer function's execution is finished, the inner function can still use the variable(s) in the lexical scope( local variable of the outer function)

   

Ex. 2 : inner functions inside methods of an object** ( private variable of closure)

```
function factory_movie(title){
    return {
        get_title : function(){
            return title;
        }
        set_title : function(_title){
            title = _title
        }
    }
}
var ghost = factory_movie('Ghost in the shell');
var matrix = factory_movie('Matrix');
```

1. in the given context, two methods are inner functions of factory_movie() 

2. the two inner functions are from the object 

3. they can access to the local variable(s) of factory_movie()  --> here, title 

4. the outer function, factory_movie(title) has a parameter, title and the parameter can be used as a local variable within the function --> two inner functions can reach the local variable(originally, a parameter), title 

5. get_title method can access to title and return it 

6. set_title method has a parameter, _title --> _title becomes title in the scope --> title resets the parameter of factory_movie(title) --> title has a new value 

7. when the function of the variable, ghost/matrix is called, it will return the object with two methods 

8. via factory_movie(title),  we made two objects and each has a different execution context ; that's, each accesses different variables(parameters) and returns different results 

   

Correct the following code to get a right result: 

The result needs to show the index from 0 to 4.  

```
var arr = [];
for(var i = 0 ; i < 5; i++) {
    arr[i] = function(){
        return i; 
    }
}
for(var index in arr){
    console.log(arr[index]());
}
```

Output: 

5

5

5

5

5

Expected: 

0

1

2

3

4

Why this happens?  

Because i doesn't refer to the value of the local variable of the outer function --> need to make i a reference to a variable/parameter of an outer function

To correct, 

a. create an outer function 

b. give it a parameter ; the parameter will be used a variable within the function's execution context 

so the code goes :

[step 1]

```;
var arr = [];
far(var i = 0 ; i < 5; i++){
    arr[i] = function(){      //create an outer function
        return function(){     //set return inner function 
            return i;
        }
    }();  // () call the outer function instantly 
}
for(var index in arr){
    console.log(arr[index]());
}
```

[step 2]

```
var arr = [];
far(var i = 0 ; i < 5; i++){
    arr[i] = function(id){      //give a parameter, id; id to be used as a local variable
        return function(){    
            return id;       //change i to id;then, id will refer to id of the outer func
        }
    }(i);                  // assign an argument, i 
}
for(var index in arr){
    console.log(arr[index]());
}
```

​        











