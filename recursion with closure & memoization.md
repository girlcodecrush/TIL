Using memoization and closure, create a function, factorial()

case 1]

```
1. factorial function w/ recursion
var factorial = function(num){
    if(num > 0){
        return num*factorial(num - 1);
    }else{
        return 1;
    }
};  

2. the factorial function w/ memoization and closure
var factorial = (function(){
  var cache = {};
  var fact = function(num){
    if(num > 0){
      var cached = cache[num - 1]|| fact(num - 1);
      var result = num * cached;
      cache[num] = result; 
      console.log(saved, result);
      return result; 
    }else{
      return 1; 
    }
  };
  return fact; 
})();
factorial(7); 
factorial(7); 
```

first output;

factorial(7);
1 1

1 2
2 6
6 24
24 120
120 720
720 5040
5040

second output:  computes faster after memorization

720 5040
5040



case 2]

```
1. fibonacci w/ recursion
var fibonacci = function(num){
    if(num < 2){
        return num ;
    }else{
        return fibonacci(num - 1) + fibonacci(num - 2);
    }
};

2. the fibonacci function w/ memoization, recursion & closure
var fibonacci = (function(){
  var cache = {};
  var fibo = function(num){
    if(num < 2){
        return num ;
    }else{
       var saved1 = cache[num - 1]||fibo(num - 1);
       var saved2 = cache[num - 2]||fibo(num - 2);
       var result = saved1 + saved2;
       cache[num] = result; 
       console.log(saved1, saved2, result);
       return result;
    }
  };
  return fibo; 
});
fibonacci(40);
fibonacci(40);
```

first output:

fibonacci(5);
1 0 1
1 1 2
2 1 3
3 2 5
5

Second output: 

fibonacci(5);
3 2 5
5

first output for fibonacci(40):

fibonacci(40);
5 3 8
8 5 13
13 8 21
21 13 34
34 21 55
55 34 89
89 55 144
144 89 233
233 144 377
377 233 610
610 377 987
987 610 1597
1597 987 2584
2584 1597 4181
4181 2584 6765
6765 4181 10946
10946 6765 17711
17711 10946 28657
28657 17711 46368
46368 28657 75025
75025 46368 121393
121393 75025 196418
196418 121393 317811
317811 196418 514229
514229 317811 832040
832040 514229 1346269
1346269 832040 2178309
2178309 1346269 3524578
3524578 2178309 5702887
5702887 3524578 9227465
9227465 5702887 14930352
14930352 9227465 24157817
24157817 14930352 39088169
39088169 24157817 63245986
63245986 39088169 102334155
102334155

second output for fibonacci(40);

fibonacci(40);
63245986 39088169 102334155
102334155

