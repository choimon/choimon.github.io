---
title: 'Shadow DOM을 활용하자'
last_modified_at: 2023-04-22T14:44
categories:
  - frontend
tags:
  - WebAPI
  - DOM
  - shadowDOM
toc: true
toc_sticky: true
---


## Shadow DOM이란
브라우저의 렌더링 엔진은 HTML과 같은 문서(document)를 파싱해서 DOM(Document Object Model) tree를 만든다. DOM 트리는 문서 내용과 구조를 나타내는 계층구조 형태로 만들어 진다. DOM API를 사용해서 DOM 트리와 그 트리가 나타내는 문서를 표현, 저장, 조작할 수 있다. 

Shadow DOM은 DOM과 같이 웹 페이지를 조작할 수 있는 API다. DOM과 Shadow DOM의 다른 점은, Shadow DOM은 컴포넌트의 구조나 CSS 스타일을 캡슐화(encapsulate)할 수 있다. Shadow DOM을 사용해서 DOM으로 생성한 메인 DOM tree 와 별개의 DOM tree를 만들 수 있는데, 이것을 "shadow tree"라고 한다. Shadow tree는 메인 DOM tree의 엘리먼트에 붙여서 사용되지만, shadow tree 외부의 엘리먼트의 구조나 스타일에 영향받지 않는다. 

Shadow DOM은 기존 DOM tree에 생성한 엘리먼트의 스타일에 영향받지 않는 엘리먼트를 만들 때 사용될 수 있다. 


## 구성요소 
![shadow_dom_tree]({{"/assets/images/posts/20230422_shadowdom_tree.png"| relative_url}})[^fn1]

- **shaodw host**  : shadow DOM이 붙게되는 일반적인 DOM 노드
- **shadow tree** : shadow DOM안에 있는 DOM 트리
- **shadow boundary** : shadow DOM 이 끝나고 일반적인 DOM이 시작되는 부분
- **shadow root**: shadow tree의 루트 노드 



## 샘플 코드 
`Element.attachShadow()`메소드를 이용해 아무 엘리먼트에 shadow root을 붙일 수 있다. 
```javascript
const shadowOpen = elementRef.attachShadow({ mode: "open" });
```

`attachShadow` 메소드 안에 들어가는 파라미터 오브젝트에 `mode`값을 넣는다.
`mode` 에는 `open`과 `closed`가 있는데, `open`인 경우에 문서 메인 페이지에 작성된 자바스크립트로 shadow DOM에 엑세스할 수 있다. 
```javascript
// closed 로 attachShadow 하면 null값이 리턴된다.
const myShadowDom = myCustomElem.shadowRoot; 
```


아래는 메인 DOM에 p 엘리먼트 1개, div 1개, p 엘리먼트 1개를 만들고, div 엘리먼트를 `shadow host`로 사용한 예제 코드다. 
`shadow root`[^fn2]와 `shadow tree`는 div 엘리먼트 아래에 생긴다.


```javascript
const pElt = document.createElement('p');
pElt.textContent = "111 this is main dom !not shadowed"; 
const styleElt = document.createElement('style'); 
styleElt.textContent = `
    p {
        color: blue; 
        background-color: lightblue;
    }
`;
document.body.appendChild(styleElt)
document.body.appendChild(pElt)

/////////////////////////////////////////////////////////////////////
/// create a shadow root and attach it to an elt. 
const shadowElt = document.createElement('div'); 
const shadowRoot = shadowElt.attachShadow({ mode: 'open'})

// create a DOM tree inside the shadow root 
const pShadowElt = document.createElement('p'); 
pShadowElt.innerHTML = `THIS IS A PARAGRAPH INSDIE THE SHADOW DOM`;

shadowRoot.appendChild(pShadowElt); 
document.body.appendChild(shadowElt)

/////////////////////////////////////////////////////////////////////

const pElt2 = document.createElement('p');
pElt2.textContent = "222 this is main dom !not shadowed"; 
document.body.appendChild(pElt2)

```



메인 DOM `p`엘리먼트에 배경과 폰트 스타일을 파란색으로 주었다. shadow DOM `p`엘리먼트에는 이 스타일이 적용되지 않은 것을 볼 수 있다. 
![shadow_dom_ex]({{"/assets/images/posts/20230422_shadowdom_ex.png"| relative_url}})<!-- {:style="height:500px;"} -->


## Shadow DOM을 보기위한 chrome 브라우저 설정
위에 개발자가 직접 넣은 Shadow DOM 뿐만 아니라, 기본적으로 제공하는 `<input type="file">` 이나 `<textarea>`와 같은 HTML 요소들도 shadow DOM을 사용한다[^fn3]. 
기본적으로는 크롬 브라우저 개발자 도구 `Elements`탭에서는 위 요소들 안의 shadow DOM 요소들까지 보여주지 않는다. 
![shadow_dom_before_setting]({{"/assets/images/posts/20230422_shadowdom_beforeset.png"| relative_url}}){:style="width:80%;"}

아래처럼 크롬 브라우저 개발자 도구 설정 페이지에서 `Show user agent shadow DOM`을 활성화 해준다.
![shadow_dom_setting]({{"/assets/images/posts/20230422_shadowdom_set.png"| relative_url}}){:style="width:80%;"}

크롬 브라우저 설정 후 위 HTML 요소들의 shadow DOM이 보인다.
![shadow_dom_after_setting]({{"/assets/images/posts/20230422_shadowdom_afterset.png"| relative_url}})


# References
[^fn1]: [mdn web docs: Using shadow DOM](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM){:target="_blank"}
[^fn2]: [mdn web docs: ShadowRoot](https://developer.mozilla.org/en-US/docs/Web/API/ShadowRoot){:target="_blank"}
[^fn3]: [코딩애플: Shadow DOM](https://codingapple.com/unit/html-31-shadow-dom/){:target="_blank"}
