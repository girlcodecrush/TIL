Execution Context 

- 함수 호출 시 생성
- Call stack에 push 
- 함수를 벗어나면(??) call stack에서 떨어져 나감 
- scope 단위로 생성
- execution context에 담긴 정보들 
  - scope내 변수, 함수 (local & global)
  - arguments 
  - caller 호출하는 함수/호출된 근원
  - this



another version of explanation:

- global code, code inside function이 실행되기 위해 필요한 환경
- 함수호출시, 해당 함수의 실행컨텍스트가 생성, 직전에 실행된 코드 블록의 실행 컨텍스트 위에 쌓임
- 함수 실행 끝나면, 함수 실행 컨텍스트 파기, 직전의 실행 컨텍스트에 컨트롤을 반환, 콜스텍에서 떨어져 나감 
- code를 실행하는데 필요한 정보를 담고 있음 
  - Variables: global, local, parameters, object's properties
  - function declarations( not, function expressions)
  - scope
  - this
- execution context는 객체 형태, 3가지 properties 소유
  - variable object - vars, function declerations, arguments..
  - scope chain - variable object & all parent scopes
  - this value - context object



***in a nutshell: call stack과 같은 말, 함수가 호출될 때 같이 생성되고 뒤따라 생성되는 것들로는 변수, 스콥, 디스, 아규먼트가 있다.  전역 및 함수 내 코드를 실행한는데 필요한 정보들을 갖고 있는 실행 환경이라고 말할 수 있다.*** 



Variable Object (VO)

실행컨텍스트 생성 후 실행에 필요한 정보를 담는 객체

실행에 필요한 정보 :

- variables
- Parameters & arguments
- function declarations( function expressions excluded)



How Execution Context Works :

```
var x = 'xxx'
function foo(){
    var y = 'yyy';
}
  function bar(){
      var z = 'zzz';
      console.log(x + y + z);
  }
  bar();
}
foo();
```

a. 전역객체 생성 --> 전역코드가 컨트롤하게 됨 --> 전역 실행컨텍스트 생성 --> 실행컨텍스트 스택에 쌓임 

b. execution context를 바탕으로 다음 처리가 실행 : scope chain 생성과 초기화, 변수객체화 실행(properties - foo & x), this value 결정

c.  함수 foo  선언

d.  변수 x 선언

e. this value 결정

f. 전역변수 x에 변수값('xxx') 할당

g.  함수 foo() 실행

h. foo()의 scope chain 생성, 변수객체화 실행(properties - bar & y), this value 결정

i. foo 함수 코드 실행 - 변수 y에 변수값('yyy') 할당 --> 함수 bar 실행

j. 새로운 execution context 생성 --> call stack에 쌓임 --> scope chain 생성 및 초기화, 변수객체화, this value 결정



