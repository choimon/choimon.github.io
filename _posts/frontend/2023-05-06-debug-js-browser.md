---
title: '브라우저에서 자바스크립트 디버그하는 방법'
last_modified_at: 2023-05-06T21:00
categories:
  - frontend
tags:
  - debug
  - js
  - debugger
  - browser
toc: true
toc_sticky: true
---





## 개요 
브라우저에서 JS 파일을 디버깅하는 방법에 대해 알아본다.

계속 추가될 예정이다.
{: .notice--info}

## Console API 
Console API는 Web API 중 일부이며, 아마 개발자들이 제일 많이 사용하는 디버깅 방법일 것이다.[^fn1]
보통 웹 콘솔에 입력한 메세지를 출력하는 `console.log` 메소드를 주로 사용한다. 
`console.log('itemId val:', itemId)`와 같이 필자는 보통 생성한 변수의 값이나 실행되는 순서를 프린트하고 싶을 때 사용한다. 
프린트된 메세지는 브라우저 개발자 도구의 `Console` 탭에서 확인할 수 있다. 

콘솔 로그는 단순이 프린팅하는 방법 말고도 색상을 달리한다던지 다양한 방법으로 프린트 할 수 있다. 이 부분은 다음에 작성하겠다. 
단순 출력하는 `console.log`말고도 `console.warn`, `console.error`, `console.assert`, `console.count`, `console.time`[^fn2] 과 같이 다양한 메소드가 존재한다.


## debugger statement (명령어)
브라우저의 개발자 도구 탭 중 하나인 `Sources` 탭에서 현재 브라우저가 실행하고 있는 JS 파일을 볼 수 있는데, `debugger` 명령어는 이 때 유용하게 사용할 수 있다. `debugger` 명령어는 자바스크립트 키워드이며, 다음과 같이 JS파일 중간에 넣을 수 있다. 

```javascript
callMyName('choichoi')

function callMyName(name) {
    debugger;   // <----- 
    console.log(`my name is ${name}`);
}
```

`callMyName` function 안에 `debugger`명령어가 들어갔다. 자바스크립트 인터프리터가 `debugger`명령어를 만나면, 해당 코드의 실행을 멈추고, 브라우저나 다른 개발 환경의 디버깅 툴을 실행(launch)한다. 만약 실행시킬 디버깅 기능이 존재하지 않는다면, 해당 명령어는 아무 영향을 끼치지 않는다. 

브라우저에서 위 파일을 실행했다고 가정한다. 브라우저의 자바스크립트 엔진은 JS 코드를 읽고 interpret을 한다. 자바스크립트 엔진이 `callMyName` 펑션 안 `debugger`명령어를 발견하고 코드 실행을 멈추고 개발자 도구 안 sources 탭 디버거가 활성화된다. 브라우저 도구의 개발자 도구를 킨 상태라면, 다음과 같은 `Sources`탭 안에서 breakpoint가 활성화된 화면이 보일 것이다. 
![debugger statement]({{"/assets/images/posts/20230506_js_debug_debugger_1.png"| relative_url}})

개발자는 디버거를 사용해서 `debugger`명령어가 활성화된 시점의 변수들의 값, call stack 그리고 라인별로 step in하여 디버깅할 수 있다. 사용자가 개발자 도구 `Sources`탭에서 직접 소스코드를 들어가 breakpoint를 지정하지 않고도 breakpoint에 걸리도록 활용할 수 있다. 





# References
[^fn1]: [mdn web docs: Console API](https://developer.mozilla.org/en-US/docs/Web/API/Console_API){:target="_blank"}
[^fn2]: [mdn web docs: Console Time](https://developer.mozilla.org/en-US/docs/Web/API/console/time){:target="_blank"}
[^fn3]: [mdn web docs: debugger statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger){:target="_blank"}
