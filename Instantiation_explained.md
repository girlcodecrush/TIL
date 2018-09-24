Instantiation

1.Functional

```
//create a function, Car, with a property called position and a method, move
var Car = function() {
//assign an object, someInstance, then put return at the end to get the object
//as an output 
    var someInstance = {};
    //set the initial value of position to 0 as shown below 
    someInstance.position = 0 ;
    //add a method,move, to the object
    someInstance.move = function(){
        this.position += 1;
    }
    return someInstance;
};
var car1 = Car();
car1.move(); 

// [object Object] {
  move: function(){
    this.position += 1 ; 
  },
  position: 1
}

Also, can assign the initial value of position by setting it as a parameter in the beginning, as shown below: 

//position as a parameter
var Car = function(position){
    var someInstance = {};
    //set the initial value to position since position is a parameter
    someInstance.position = position;
    someInstance.move()= function(){
        this.position += 1 ;
    }
    return someInstance;
};
var car1 = Car(3); 
car1.move();
//[object Object] {
  move: function(){
    this.position += 1 ; 
  },
  position: 4
}
```

2.Functional Shared

```
//3.create a f(), extend, to compile someInstance{}, which doens't have a method
//and someMethods{}, which has a method within 
//then, combine them inside the Car function
var extend = function(to, from){
  for(var key in from){
    to[key] = from[key]; 
    //the method of the obj2 shares its memory address with properties of obj1
    //for the instances created 
  }
};

//2. create an obj that contains a method
var someMethods = {};
//put the method inside the obj
someMethods.move = function(){
  this.position += 1;
}

//1. declare a f(),Car, then add position as a property of the obj
var Car = function(position){
  var someInstance = {
    position:position, 
  };
  //place the extend function in the Car function-> increases the effiency of 
  //memory (see the image below)
  extend(someInstance, someMethods);
    
    return someInstance;
};

var car1 = Car(10);
var car2 = Car(11);
car1.move();
car2.move();
//
car1
[object Object] {
  move: function(){
  this.position += 1;
},
  position: 11
}
car2
[object Object] {
  move: function(){
  this.position += 1;
},
  position: 12
}

```

From the memory efficiency standpoint, functional shared occupies less space in memory then functional

![](/Users/andreakim/Desktop/Screen Shot 2018-09-24 at 1.36.28 PM.png)

In functional, 

- each time a new instance is created, it assigns the method to someInstance 

- Individual instances occupy the spaces as many as the number of methods

  

In functional shared, 

- each instance only refences to the memory addess of the method in obj2
- the memory efficiency improves 

3.Prototypal

```
var someMethods = {};
someMethods.move = function(){
  this.position += 1;
};

var Car = function(position){
  //instead of creating an extend function, using Object.create() 
  //create an obj that includes a specific obj as a prototype
  // in var someIsntance = {}, drop {} and put Object.create(someMethods)
  var someInstance = Object.create(someMethods);
  someInstance.position = position;
  
  return someInstance;
};

var car1 = Car(10);
var car2 = Car(11);

car1.move();
car2.move();

//car1
[object Object] {
  move: function(){
  this.position += 1;
},
  position: 11
}
car2
[object Object] {
  move: function(){
  this.position += 1;
},
  position: 12
}
```

4.Pseudoclassical

**used the most among the instantiation patterns*

```
var Car = function(position){
    this.position = position;      //*this refers to Car 
};
//make the method a prototype  
Car.prototype.move = function(){
  this.position += 1;  
};
//put the new operator 
var car1 = new Car(10);
var car2 = new Car(11);
car1.move();
car2.move();

//car1
[object Object] {
  move: function(){
  this.position += 1;
},
  position: 11
}
car2
[object Object] {
  move: function(){
  this.position += 1;
},
  position: 12
}
```

```
Car(10)
undefined

new Car(10)
Car {position: 10}
position: 10
__proto__:
move: ƒ ()
constructor: ƒ (position)
__proto__: Object

car1
Car {position: 11}
position: 11
__proto__:
move: ƒ ()
constructor: ƒ (position)
__proto__: Object
```

















