[CW]Your task is to sort a given string. Each word in the String will contain a single number. This number is the position the word should have in the result.

Note: Numbers can be from 1 to 9. So 1 will be the first word (not 0).

If the input String is empty, return an empty String. The words in the input String will only contain valid consecutive numbers.

For an input: "is2 Thi1s T4est 3a" the function should return "Thi1s is2 3a T4est"

Test.assertEquals(order("is2 Thi1s T4est 3a"), "Thi1s is2 3a T4est")
Test.assertEquals(order("4of Fo1r pe6ople g3ood th5e the2"), "Fo1r the2 g3ood 4of th5e pe6ople")
Test.assertEquals(order(""), "")

```
function order(words){
  // ...
  if(words.length === 0 ){
    return "";
  }
  
  var arr = words.split(" ");
  var wordsArr = [];
  var numArr = [1,2,3,4,5,6,7,8,9];
  
  for(var i = 0 ; i < numArr.length ; i++){
    for(var j = 0 ; j < arr.length ; j++){
      if(arr[j].includes(numArr[i])){
        wordsArr.push(arr[j]);
        newStr = wordsArr.join(" "); 
      }
    }
  }
  return newStr; 
}
```



[another solution] using regular expressions

```
function order(words){
  
  return words.split(' ').sort(function(a, b){
      return a.match(/\d/) - b.match(/\d/);
   }).join(' ');
}   
```

