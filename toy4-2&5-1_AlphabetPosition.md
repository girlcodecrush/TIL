4-2

In this exercise, you are required to, given a string, replace every letter with its position in the alphabet. 

 If anything in the text isn't a letter, ignore it and don't return it. a being 1, b being 2, etc.

Example 1: alphabetPosition("The sunset sets at twelve o' clock."); => "20 8 5 19 21 14 19 5 20 19 5 20 19 1 20 20 23 5 12 22 5 15 3 12 15 3 11"

```
var userInput = prompt("Enter a String:");
  
var alphabetPosition = function(str) {
  // Your code here
  var newStr = str.toUpperCase();
  var obj = {A:1, B:2, C:3, D:4, E:5, F:6, G:7, H:8, I:9, J:10, K:11, L:12, M:13, N:14, O:15, 
  P:16, Q:17, R:18, S:19, T:20, U:21, V:22, W:23, X:24, Y:25, Z:26};
  
  var pstStr = "";

  for(var i = 0 ; i < newStr.length ; i++){
    for(var key in obj){
      if(newStr[i]=== key){
        pstStr += obj[key] + " ";
      }
    }
  }
  return pstStr; 
};

console.log(alphabetPosition(userInput));
```

Input: Coding is fun

Output: 3 15 4 9 14 7 9 19 6 21 14 



5-1

MultiplicativePersistence

 Have the function MultiplicativePersistence(num) take the num parameter being passed which will always be a positive integer and return its multiplicative persistence which is the number of times you must multiply the digits in num until you reach a single digit. 

For example: if num is 39 then your program should return 3 because 3 * 9 = 27 then 2 * 7 = 14 and finally 1 * 4 = 4 and you stop at 4. 

```
var userInput = prompt("Enter a num:");

var MultiplicativePersistence = function(num) {
  
  var numStr = num.toString();
  var arr = numStr.split("");

  //base case
  if(numStr.length === 1){
    return 0;
  }
  else if (numStr.length > 1){
    var multiNum = arr.reduce(function(accum, n){
      return accum * n;
    }); 
    //call the func itself recursively until length equals 1 
    // +1 is for counting every time the func is executed 
    // counted once when the func first executed and passed the count 
    // over to the next execution - count is accumulated 
    return MultiplicativePersistence(multiNum) + 1;
  } 
};

console.log(MultiplicativePersistence(userInput));
```

