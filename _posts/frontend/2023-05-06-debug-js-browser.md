---
title: '브라우저에서 자바스크립트 디버그하는 방법'
last_modified_at: 2023-05-12T00:05
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

### console 메세지 포맷하기
자바의 `String.format`에서 `%s`와 같은 specifier처럼 콘솔 메세지를 formatting specifier를 사용해 포맷할 수 있다. 

| Specifier    | Output      |  Example   |  
|:-------------|:------------|:---------------|
| `%s`         |  값을 string으로 포맷          |      | 
| `%i` 또는<br/>`%d`  |  값을 integer로 포맷          |        | 
| `%f`     |  값을 floating point 값으로 포맷         |        | 
| `%o`     |  값을 expandable DOM 엘리먼트로 포맷         |        | 
| `%O`     |  값을 expandable 자바스크립트 object로 포맷         |        | 
| `%c`     |  출력 문자열에 CSS 스타일 값을 적용한다. 값은 두번째 파라미터로 전달받는다.   |        | 


위 specifier를 사용한 예제는 다음과 같다.
``` javascript
const petName = "초이";
const petAge = 100;
const petInfo = {
    owner: "choichoi",
    nationality: 'korea',
    favoriteFood : "lettuce"
}; 
const googleLogoElt = document.querySelector("img.lnXdpd");

console.log('우리집 펫 이름은 %s야. 나이는 %i살이고, 더 자세한 정보는 아래와 같아 %o', petName, petAge, petInfo);

// typeof googleLogoElt 결과는 object 다.
console.log('구글 로그 돔 엘리먼트는 %O', googleLogoElt); 
console.log('구글 로그 돔 엘리먼트는 %o', googleLogoElt); 
console.log('구글 로그 돔 엘리먼트는 %s', googleLogoElt); 
console.log('구글 로그 돔 엘리먼트는 %i', googleLogoElt); 

```

![custom console.log format]({{"/assets/images/posts/20230511_js_debug_debugger_3.png"| relative_url}})

전달한 specifier에 따라 프린트된 구글 로고 엘리먼트가 다른 것을 볼 수 있다. 

### console 메세지에 스타일 주기 
`console.log()`를 여러번 호출하다보면, 콘솔 창에서 내가 원하는 콘솔 메세지를 찾기 어려울 때가 있다. 이때 사용하면 유용할 것 같은 방법을 소개한다. 콘솔 메세지를 단순히 문자열을 전달하여 프린트하는 것이 아니라, 폰트 크기와 색상같이 다양한 스타일을 줘서 프린트할 수 있다. [^fn3]
```javascript 
const style = 'background-color: darkblue; color: white; font-style: italic; border: 5px solid hotpink; font-size: 2em;'
console.log("%c커스텀 스타일이다!", style);
```
위 `style`변수에 선언한 것처럼, 다크 블루 배경에, 핫핑크 테두리와 italic된 2em크기의 흰색 폰트를 지정해서, 다른 콘솔 메세지와 다르게 표시할 수 있다. 콘솔에서 보여지는 결과는 다음과 같다. 
![custom console.log style]({{"/assets/images/posts/20230511_js_debug_debugger_2.png"| relative_url}})


## debugger statement (명령어)
브라우저의 개발자 도구 탭 중 하나인 `Sources` 탭에서 현재 브라우저가 실행하고 있는 JS 파일을 볼 수 있는데, `debugger` 명령어는 이 때 유용하게 사용할 수 있다. `debugger` 명령어는 자바스크립트 키워드이며, 다음과 같이 JS파일 중간에 넣을 수 있다.[^fn4]

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
[^fn3]: [chrome developer devtools](https://developer.chrome.com/docs/devtools/console/format-style/){:target="_blank"}
[^fn4]: [mdn web docs: debugger statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger){:target="_blank"}
