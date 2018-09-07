1. Return key:value pairs in the given object 

   ```
   var sports = {
       soccer: {member:11, 
                time: 90},
       basketball:{member:5,
                   time: 48}
   };
   
   or 
   var sports = {
       soccer:{step: {value:11}, 
               time: 90
               },
       basketball: {member:5,
                    time: 48
                    }
   }
   
   or
   var sports= {
       soccer: {step: {value:11}},
                time: 90
                },
       basketball: {memeber:5,
                    time: 48
                    teams: {A:'NY',
                            B:'Colorado',
                            C:'Wisconsin'}
                    }         
   }
   ```

   ```
   // without calling a function recursively
   function showValues(sports){
       for(var type in sports ){
         var obj = sports[type];  
       
         for(var key in obj){
           console.log(key + ":" + obj[key]);
           
           /* var obj2 = obj[key] }
              for(var prop in obj2){
                  console.og(prop + ":" + obj2[prop])
              } .... and so on and on and on.. */
         }
       } 
   }
   // in this case,if the object has a deeper layers of objects within itself, more for-in loop will be necessariy used to go deeper into the layers. Recursion is the key to solve such need of repetitive use of for-loops
   
   
   //with recursion
   function showValues(sports){
       for(var type in sports){
           var obj = sports[type];
       }
       
       typeof obj === 'object'? showValues(obj): console.log('None'); 
   }
   
   //with recursion, for-loop is no longer needed repetitively
   //no matter how many layers of objects within the original object, recursion loops through the entire layers deep inside 
   ```

   ```
   output:
   member:11 (or value:11)
   time:90
   member:5
   time:48
   (A: 'NY'
    B: 'Colorado'
    C: 'Wisconsin')
   ```

   

2. for loop v. recursion 

   return numbers from the given number to 1 

   ```
   //without a function called recursively 
   function countDown (num){
     for(var i = num ; i >= 1 ; i--){
         console.log(i);
     }
   }
   countDown(5);
   
   //with recursion 
   function countDown(num){
       console.log(num);
       num--;
       
       if(num <= 0){
          return undefined; 
       }
       
       countDown(num);
   }
   countDown(5);
   //  recursion replaces the scope of a variable in for-loop with the rest, action(console.log), increment/decrement, stated in advance 
   ```

   ```
   output: 
   5
   4
   3
   2
   1
   ```

   