```
//with IIFE -> take a close look at what Utils means 
var Utils = (function(){
  return {
    test: function(){
      console.log('Hooray!');
    },
    test2:function(){
      console.log('YEAH!');
    }
  };
})();
```

What does Utils refer to?

```
{test: ƒ, test2: ƒ}
test: ƒ ()
test2:ƒ ()
__proto__:Object
```

```
output:
Utils.test();
Hooray!

Utils.test2();
YEAH!
```

Without IIFE : 

```
var Utils = function(){
  return {
    test: function(){
      console.log('Hooray!');
    },
    test2:function(){
      console.log('YEAH!');
    }
  };
};
```

The Utils variable is a function. It  means;

```
ƒ (){
  return {
    test: function(){
      console.log('Hooray!');
    },
    test2:function(){
      console.log('YEAH!');
    }
  };
}
```

Utils() refers to:

```
{test: ƒ, test2: ƒ}
test: ƒ ()
test2:ƒ ()
__proto__:Object
```

Utils().test ;

```
ƒ (){
      console.log('Hooray!');
    }
```

```
Utils().test();
Hooray!

Utils().test2();
YEAH!
```

