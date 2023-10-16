---
title: 'OS 다크모드 감지해서 CSS 적용하기 (feat.vue)'
last_modified_at: 2023-10-16T22:36
categories:
  - frontend
tags:
  - js
  - css
  - media query
toc: true
toc_sticky: true
---

## CSS media query

```css
@media (prefers-color-scheme: dark) {
  body {
    background: #000;
  }
}
@media (prefers-color-scheme: light) {
  body {
    background: #fff;
  }
}
```

OS가 다크모드로 세팅되어있으면 `@media (prefers-color-scheme: dark)` 안 css가,[^fn1] <br/>
라이트모드라면 `@media (prefers-color-scheme: light)` 안 css가 적용된다.


Safari/Webkit 은 아래를 사용한다. 
```css
@media (prefers-dark-interface) { background: #000; }
@media (prefers-light-interface) { background: #fff; }
```

## JS
```javascript
if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
    // dark mode
    console.log(`dark mode!`);
}
```
위 코드를 보면 먼저 브라우저가 `matchMedia`를 지원하는지 체크하고, `prefers-color-scheme: dark` 라는 스트링이 있는지 매치시켜본다. 
이런 스트링이 존재한다면 다크모드라는 걸 알 수 있다.[^fn2]

`matchMedia()`는 `MediaQueryList`객체를 반환한다. 이 객체를 사용해서 현재 보여주고 있는 문서(document)가 `media query` 문자열을 매칭하는지 알 수 있다.
[^fn3]
`MediaQueryList` 객체는 문서에 적용된 `media query`정보를 저장하고 있다. `MediaQueryList`객체는 부모 interface가 `EventTarget`이기 때문에, 
media query 상태가 변경되면 listener에 알려주는 기능도 담당한다. [^fn4]
변경을 감지하고 액션을 취하고 싶다면 아래와 같이 사용한다.
```javascript
// dark mode 변경 감지
window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', e => {
  const newColorScheme = e.matches ? "dark" : "light";
});
```


## Vue3에서 OS,디바이스 다크모드 감지하기
위 JS 예제를 활용해서 Vue3에서 OS 다크모드를 감지해보자.
```html
<script setup lang="ts">
import { ref, onBeforeMount } from 'vue';

const isDarkMode = ref(false);

onBeforeMount(() => {
  if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
    console.log(`dark mode`);
    isDarkMode.value = true; 
  }
})
</script>

<template>
  <div :class="isDarkMode? 'dark-mode' : 'light-mode'">
      <p>User Device in {{ isDarkMode ? 'Dark Mode' : 'Light Mode' }}</p>
  </div>
</template>
```

# References
[^fn1]: [mdn: prefers-color-scheme](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme){:target="_blank"}
[^fn2]: [vue: reactivity fundamentals ](https://stackoverflow.com/questions/50840168/how-to-detect-if-the-os-is-in-dark-mode-in-browsers){:target="_blank"}
[^fn3]: [mdn: matchMedia()](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia){:target="_blank"}
[^fn4]: [mdn: MediaQueryList](https://developer.mozilla.org/en-US/docs/Web/API/MediaQueryList){:target="_blank"}
[^fn5]: [fontawesome: lightdarkmode](https://fontawesomeicons.com/fa/vue-js-check-whether-user-device-in-dark-or-light-mode){:target="_blank"}
