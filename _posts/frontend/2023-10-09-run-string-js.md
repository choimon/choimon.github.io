---
title: 'JS 문자열 실행 (eval, new Function)'
last_modified_at: 2023-10-09T16:01
categories:
  - frontend
tags:
  - js
  - run string as js
toc: true
toc_sticky: true
---


# 개요
간혹 문자열로 저장된 js 코드를 실행해야 할 때가 있다. 이럴 때는 `eval`이나 `new Function`을 사용할 수 있다.
각 방법을 알아본다. 
필자같은 경우, DB에 저장된 string 값을 실행해야될 때가 있었어서 이런 방법을 찾게 되었다. 결론적으로는, 사용하지 않고 DB에 들어가는 데이터를 세분화했다. 


# eval
`eval`[^fn1]은 문자열을 js 코드로 실행한다. 


심플하게 alert message하는 코드 예제다:
```javascript
const alertJsString = `alert("hello world")`;
eval(alertJsString);
```


함수를 문자열로 저장한 `jsString`변수를 `eval`로 실행하는 예제다:
```javascript
let outSideVar = 'initialName';

function outSideStr(name) { 
    console.log(`my name is ${name}`);
}

const jsString = `function sayHello(name) {
    outSideVar = name;
    console.log("function inside string called... Hello " + name);
    console.log("outSideVar: " + outSideVar);
    console.log(\`calling outSideStr()...\`, outSideStr);
    outSideStr(\`\${name}!!!!!!\`);
}`;

eval(jsString);
sayHello('minjeong');
console.log(`outSideVar~~~`, outSideVar);
```

콘솔 결과 값이다
```TEXT
function inside string called... Hello minjeong
outSideVar: minjeong
calling outSideStr()... ƒ outSideStr(name) {
    console.log(`my name is ${name}`);
}
my name is minjeong!!!!!!
outSideVar~~~ minjeong
```

위 예제에서는 문자열 밖에서 선언된 함수`outSideStr`을 문자열 안에서 실행할 수 있고, 변수 `outSideVar`값을 변경할 수 있는걸 볼 수 있다. 냄새가 나지 않는가? 위험하고 쎄한 냄새

`eval`은 mdn 사이트[^fn2]에서 경고메세지와 함께 사용하지 말라고 한다.
> - eval() executes the code it's passed with the privileges of the caller. If you run eval() with a string that could be affected by a malicious party, you may end up running malicious code on the user's machine with the permissions of your webpage / extension. More importantly, allowing third-party code to access the scope in which eval() was invoked (if it's a direct eval) can lead to possible attacks that reads or changes local variables.
> - eval() is slower than the alternatives, since it has to invoke the JavaScript interpreter, while many other constructs are optimized by modern JS engines.
> - Modern JavaScript interpreters convert JavaScript to machine code. This means that any concept of variable naming gets obliterated. Thus, any use of eval() will force the browser to do long expensive variable name lookups to figure out where the variable exists in the machine code and set its value. Additionally, new things can be introduced to that variable through eval(), such as changing the type of that variable, forcing the browser to re-evaluate all of the generated machine code to compensate.
> - Minifiers give up on any minification if the scope is transitively depended on by eval(), because otherwise eval() cannot read the correct variable at runtime.


간략하게 번역하자면, eval()은 eval을 실행하는 caller의 권한을 가지고 실행된다. (위 예제 코드같은경우는 eval을 실행한 스코프에 권한이 있어서, outSideStr 함수를 실행할 수 있었나보다)
`eval`은 대체제들보다 느리다. eval은 JS 인터프리터를 실행하는데 다른 대체제들은 JS 엔진 최적화가 되어있어있다.
요즘 JS 인터프리터는 JS를 기계 코드로 변환한다. 기계코드로 변환한다는 말은 기존 변수명 개념이 사라진다는 말이다 . eval()을 사용하면, 브라우저는 eval() 변수명을 찾는 데 길고 비싼 과정을 거쳐야한다. 브라우저는 변수명이 기계코드 어디에서 존재하는지 찾고 해당 변수 값을 세팅해줘야 한다. 
Minifier들은 eval()을 사용하는 scope을 minify하지 않는다. 그렇게 하지 않으면, eval()이 런타임에 알맞은 변수를 찾지 못하기 때문이다.



만약 eval이 실행하는 코드가 악의적인 코드를 담고 있다고 상상하자. eval은 XSS 공격에 취약하다. 일반 사용자가 임의로 입력한 값을 eval로 실행하면 안된다. 

그렇다면 어떤 다른 대체제가 있는 것인가? 


# new Function()
`new Function()`은 `eval`과 비슷하게 문자열을 js 코드로 실행한다. 
앞 parameter는 함수의 parameter이고, 마지막 parameter는 함수의 body이다. 
새로운 Scope에 실행되기 때문에, `eval`과 다르게 함수 내부 Scope에만 접근과 수정이 가능하다. 내부에서 선언된 변수는 선언한 `Function`내에서만 유효하다. 

```javascript
// 인수 없는 경우 
let hello = new Function('alert("Hello")');
hello(); // Hello

// 인수 두 개인 경우
const add = new Function('a', 'b', 'return a + b;');
// const add = new Function('a,b', 'return a + b'); // 쉼표로 구분가능
// const add = new Function('a , b', 'return a + b'); // 쉼표와 공백으로 구분가능
console.log(add(2, 3)); // 5


```



# References
[^fn1]: [mdn: eval](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval){:target="_blank"}
[^fn2]: [mdn: never use eval](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#never_use_eval!){:target="_blank"}
[^fn3]: [tistory: 지니의 기록](https://cocobi.tistory.com/77){:target="_blank"}

