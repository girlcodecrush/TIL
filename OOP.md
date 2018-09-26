Object-Oriented Programming

- use of constructor functions
- to make constructor functions, should be well aware of new and this
- the new keyword when referring to the constructor function
- the this keyword for the last called function parent object

 -> if it has no parent object, Window will be its parent object

-> can bind a function to this keyword using Function.bind()

[Reference]   ——————————  

MDN: Function.prototype.bind()

the bind() function creates a new bound fucntion(BF) ; a BF is an exotic function object that wraps the original function object 

the simplest use of bind() is to make a functionthat is called with a particular this value

```
this.x = 9;    //this refers tp global Window object
var module = {
    x:81,
    getX: function() {
        return this.x;
    }
};
module.getX();    //81

var retrieveX = module.getX; 
retrieveX(); //9 - why? retrieveX() gets invoked at the global scope


//Create a new function with 'this' bound to module
var boundGetX = retrieveX.bind(module);
boundGetX();   //81 - boundGetX() can access to the property x in module thanks to bind
```

```
var Button = function(content) { 
  this.content = content;
};
Button.prototype.click = function() {
  console.log(this.content + ' clicked');
};

var myButton = new Button('OK');
myButton.click();

var looseClick = myButton.click;
looseClick(); // not bound, 'this' doesn't refer to Button - it's the global object

var boundClick = myButton.click.bind(myButton);
boundClick();  // bound to Button thanks to 'bind'; this is Button

output:
OK clicked
undefined clicked
OK clicked
```

​                                                            —— ——— ——— ——— ——  

The code below returns a new object, Bob, as a result of the constructor function. The function has the properties Bob.firstName, Bob.lastName, Bob.fullName, and Bob.eMail.

```
var ConstructorForPerson = function(first, last, email){
    this.firstName = first;
    this.lastName = last;
    this.fullName = first + " " + last;
    this.eMail = email;
}
var Bob = new ConstructorForPerson("bob", "brown", 
"bob122099@gmail.com");

console.log(Bob.eMail);
//evals "bob122099@gmail.com"
```

-inside of the constructor function, each line ends with a semicolon



Example: code a representation of Google as a business taking the object, the properties, and the methods as shown below

- The object  - the building/ management
- the properties - the employees 
- the methods - what Google does, what the employees do 

Step 1 : Create a global object, Google ;  it containes another object for employees, which contains more objects for each role and its individual employees 

Step 2: make a constructor using a normal function that has 7 properties - name, role, phone, idNumber, working, and hours

Step 3: add 1 method, clockInOut() --> this method looks at the working property to update hours property

Step 4: update the Google object with the "NewEmployee() constructor function"

Using the new operator, a constructor function, and this : 

```
var google = { //create {google}
employees: {   
      management: {
      },
      developers: {
      },
      maintenance: {
      }  
   }
};
//make a contructor function with properties - fullName, role, phone, idNumber, working,
//hours, and clockInOut(method)
var NewEmployee = function(fullName, role, phone, idNumber){
  this.fullName = fullName;
  this.role = role;
  this.phone = phone;
  this.idNumber = idNumber; 
  this.working = false;
  this.hours = [];
  
  NewEmployee.prototype.clockInOut = function(){
    if(this.working){
      this.hours[this.hours.length - 1].push(Date.now());
      this.working =false;
      return this.fullName + " clocked out at " + Date.now();
    }else{
      this.hours.push([Date.now()]);
      this.working = true;
      return this.fullName + " clocked in at " + Date.now();
    }
  }
 };

var newEmployee1 = new NewEmployee('James Coopers', 'maintenance', '135792468', 'jc1234');
newEmployee1.clockInOut();

//output
"James Coopers clocked in at 1537943237388"
newEmployee1.clockInOut();
"James Coopers clocked out at 1537943951102"
newEmployee1.clockInOut();
"James Coopers clocked in at 1537943961901"
newEmployee1.clockInOut();
"James Coopers clocked out at 1537943973139"
newEmployee1.clockInOut();
"James Coopers clocked in at 1537943976269"
newEmployee1.clockInOut();
"James Coopers clocked out at 1537943978296"

//examine newEmployee1:
newEmployee1
NewEmployee {fullName: "James Coopers", role: "maintenance", phone: "135792468", idNumber: "jc1234", working: false, …}
fullName: "James Coopers"
hours: Array(3)
0: (2) [1537943237388, 1537943951102]
1: (2) [1537943961901, 1537943973139]
2: (2) [1537943976269, 1537943978296]
length: 3
__proto__: Array(0)
idNumber: "jc1234"
phone: "135792468"
role: "maintenance"
working: false
__proto__: Object
```

without using the new operator and this within the constructor function

```
var google = { //create {google}
employees: {
     
      management: {
      },
      developers: {
      },
maintenance: {
      }
     
    },
  
    NewEmployee: function(name, role, phone, idNumber) { 
//create NewExployee()
      
      var newEmployeeData = {
        name: name,
        role: role,
        phone: phone,
        idNumber: idNumber,
        working: false,
        hours: [],
       }
     //make new object to append to google.employees[role]
    
    google.employees[role][name] = newEmployeeData;
    //assign object to google.employees[role][name]
return  google.employees[role][name];
  //return the new object directly from google.employees[role][name]
      
  }//end NewEmployee
  
} //end {google}
google.NewEmployee("james", "maintenance", "2035555555", "1234521"); 
//create {google:employees:maintenance["james"]} from NewEmployee()
google.employees["maintenance"]["james"].clockInOut = function() { //create clockInOut() - default false
  
       if(this.working) {
         this.hours[this.hours.length - 1].push(Date.now());
         this.working = false;
         return this.name + " clocked out at " + Date.now(); 
       }
       else{
         this.hours.push([Date.now()]);
         this.working = true;
         return this.name + " clocked in at " + Date.now(); 
       }
return "Error" //if above doesn't work, "Error" 
}
google.employees["maintenance"]["james"].clockInOut(); //call clockInOut()
```















