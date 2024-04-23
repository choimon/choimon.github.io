---
title: 'ResizeObserver loop completed with undelivered notifications'
last_modified_at: 2024-04-23T23:50
categories:
  - frontend
tags:
  - ResizeObserver
  - WebAPI
toc: true
toc_sticky: true
---


현재 Vue3 App & Antd 컴포넌트를 사용해서 개발하고 있다. 
로컬 개발환경에서 개발하는 중에 아래와 같은 webpack devserver overlay에 runtime에러가 떴었다: 
```
Uncaught runtime errors: 

ResizeObserver loop completed with undelivered notifications. 
at handleError...
```

사실 내가 개발하던 부분은 아니고, 팀원분이 개발하는 부분인데 위와같은 에러가 뜬다고 해서 같이 확인해본 것이다. <br/>
위와같은 에러가 ant-d의 a-drawer 컴포넌트로 drawer 를 구현하는데 width/size prop을 전달하면서, 안에 a-tab 탭 컴포넌트가 들어가면 발생하는 걸 확인했다. a-drawer에 기존 다른 컴포넌트를 넣으면 발생하지 않는 걸 확인했다.

왜!! 왜 발생하는가. resizing 일어나면서 에러가 생기는 것 같아서, resizing이 안 일어나도록 a-drawer나 a-tab 안 class를 이리저리 고정 width, height값을 줘보기도 했다. 그 중 tab 안 div class의 width,height을 지정하니깐 더 이상 일어나지않았지만, tab 안에 내용을 미리 알 수 없어 값을 고정하기가 애매했다. 다른 해결책이 필요했다. 

우선 이 에러가 언제 발생되는지 알아보았다. 

## 에러 원인 

- ResizeObserver가 1개의 animation 프레임안에 모든 resize 이벤트(observations)를 deliver 못할 때 발생된다.[^fn3]

- 우선, ResizeObserver 가 어떻게 작용하는지 알아야한다. mdn 사이트[^fn1]에 따르면, resize 이벤트는 paint가 일어나기 전에 불러진다 (즉, 사용자한테 해당 프레임이 제공되기 전이다). 스타일, 레이아웃이 다시 evaluate되고, 이 과정에서 resize이벤트가 더 일어날 수 있다. 이렇게 계속 resize 이벤트가 호출되다 보면, cyclic dependencies (의존성 순환)으로 인해 inifinite loop이 발생될 수 있는데, 이때 DOM상에서 깊숙하게(deep) 위치한 엘리먼트들만 processing이 된다. DOM상에 깊숙하게 위치 하지 않은 엘리먼트의 resize 이벤트는 그다음 paint로 미뤄지며, 이때 Window 오브젝트에 아래 메시지와 함께 에러 이벤트가 발생된다.

ResizeObserver loop completed with undelivered notifications.

- 이렇게 함으로서, 발생할 수 있는 성능 이슈나 빠르게 계속 일어나는 resize 이벤트로 인해 생기는 무한 루프를 방지할 수 있다. 
또한, 계속 화면이 변경되는 것이 아니라, 사용자 경험도 더 자연스럽게 보이고, resize 이벤트를 과도하게 계산하게되면 브라우저가 멈출 수 있는데, 이런 현상도 방지한다.


- 내 코드로 돌아와서, 현재 사용하고 있는 ant design vue 버전의 코드를 git clone받아서 `new ResizeObserver` 를 호출하는 부분을 찾아봤다. tab 컴포넌트 안 tab navlist 쪽에 resizeobserver를 등록하는 걸 확인했다. 이 부분 class를 고정하면 resize 오류가 발생안했던 것과 일치했다. 


## 에러 해결 
- 여러 글들을 읽어봤는데, 저 에러가 발생하지 않도록 수정한다기보다 (물론 고정값을 주면서 해결한 사람들도 있었다)[^fn2], 
에러가 심각하지않다고 판단해 webpack devserver overlay에 보여지지않도록 설정하는 경우를 많이 봤다. 
- webpack config 

```
module.exports = {
  //...
  devServer: {
    client: {
      overlay: {
        runtimeErrors: (error) => {
          if (error.message === 'ResizeObserver loop limit exceeded') {
            return false;
          }
          return true;
        },
      },
    },
  },
};
```

- 그래도 진짜로 그냥 devserver overlay화면상에 에러만 안뜨게 해도되는건지 찝찝해서, 다른 글들을 찾아봤다.
ResizeObserver github 이슈에 이런 에러는 대부분 괜찮다(benign)고 한 코멘트를 봤다. "benign"이란 표현을 썼는데, 브라우저 페이지가 멈추지 않을 것이라는 뜻으로 해석했다. 


>"loop_limit exceeded" notification happens when ResizeObserver cannot deliver observations.
>Here is a simplified explanation:
>
>Here, height of #container depends on height of #target. If you are observing #container, and change #target height inside RO callback, you'll hit the error. #target resize will change #container height, which will trigger RO observation, which cannot be delivered because RO has already delivered notifications at this depth.
>This error is often benign. Seeing many of them would be worrysome, and might indicate an infinite resize loop.


- 물론 저런 에러가 여러번 발생하지 않거나, 실제로 ResizeObserver라는 코드를 사용하고 있지 않다면 무시해도된다는게 전반적인 의견이다. 

- 우선 개발할때 위 devserver 컨피그를 적용해서 하는게 어떤지 고민중이다.



# References
[^fn1]: [mdn web docs: ResizeObserver](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver){:target="_blank"}
[^fn2]: [SENTRY: React: ResizeObserver loop completed with undelivered notifications](https://sentry.io/answers/react-resizeobserver-loop-completed-with-undelivered-notifications/){:target="_blank"}
[^fn3]: [stackoverflow: ResizeObserver - loop limit exceeded](https://stackoverflow.com/questions/49384120/resizeobserver-loop-limit-exceeded){:target="_blank"}
[^fn4]: [github-issue: ResizeObserver#observe() firing the callback immediately results in infinite loop #38](https://github.com/WICG/resize-observer/issues/38#issuecomment-422126006){:target="_blank"}
[^fn5]: [github-wicg-issue: Which strategy for preventing infinite loop when delivering resize notifications?](https://github.com/WICG/resize-observer/issues/7){:target="_blank"}
[^fn6]: [github-ant-design-issue: ResizeObserver loop limit exceeded](https://github.com/ant-design/ant-design/issues/26621){:target="_blank"}
