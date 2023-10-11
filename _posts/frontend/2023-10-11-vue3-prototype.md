---
title: 'Vue3에서 전역변수 사용하기 (feat. Vue2 prototype)'
last_modified_at: 2023-10-11T01:00
categories:
  - frontend
tags:
  - vue3
  - vue2
toc: true
toc_sticky: true
---


# 개요
Vue2 prototype 처럼 Vue3에서 전역변수를 사용하는 방법을 알아보자. 

## 라떼는 말이야 Vue2 prototype
Vue2 쓸 때는,아래처럼  axios method들을 묶은 오브젝트를 prototype[^fn1]에 등록해서 전역변수처럼 사용했다.
```javascript
// api.js
const restApi = {
  get(url, param = {}){
    return axios.get(url, param);
  },
  //...
}

Vue.prototype.$api = restApi;

//...
// component안에서 실제사용은 아래처럼 콜하면된다.
this.$api.get("/api/v1/users").then((res) => {
    console.log(res);
});
```

위 `api.js` 코드를 `plugin`으로 한번 감싸주고 `main.js`에서 `use`해주면 얼마나 깔꼼하게여..


# 선생님 여긴 Vue3인디요
Vue3 Composition API 컨셉을 제대로 이해하기도 전에 바로 실무 투입 되었었다. 
Vue2에서 사용하던 방식을 그대로 쓰고 싶었다. 이것은 마치 몽골에 건너간 한국인이 한국에서 먹던 음식을 먹고 싶어하는 것과 같았다.

돌고 돌아 답을 찾았는데, 역시 이번에도 깨닫는다. 베이스를 탄탄하게 하자. 바로 설명간다.
Composition API 사용자지만, Options API에서 유용해보이는 것도 있어서 둘다 설명한다.

Vue3에서는 `app.config.globalProperties`에 전역 변수를 선언할 수 있다. 공식문서에 따르면 prototype을 대체하는 애라고 한다.
```javascript
import { createApp } from "vue";
import App from "./App.vue";
import api from "./api";

const app = createApp(App);
app.use(api);
app.mount("#app");

// api를 글로벌 변수에 넣음
app.config.globalProperties.api = api; 
```

## Options API
Options API에서는 `this`를 사용할 수 있기 때문에, `this.api`로 사용할 수 있다.
사용하는 예제: 
```javascript
<template>
//...
</template>
<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "Hello",
  mounted() {
    console.log(this.api);
    this.api.get("/api/v1/users").then((res) => {
      console.log(res);
    });
  }
});
</script>
```
## Composition API

### globalProperties 꺼내오기
Composition API에서는 호락호락하지 않다. 
`app.config.globalProperties`에 접근하기 위해서는 `getCurrentInstance()`를 사용해야한다.[^fn2]

```javascript
<script setup>
const app = getCurrentInstance().appContext.app;
app.config.globalProperties.api.get("/api/v1/users").then((res) => {
    console.log(res);
});
</script>
```
(위 방법 말고도 `getCurrentInstance()`에서 `proxy` 꺼내와서 `proxy.api` 하시는 분 글도 봤는데 나는 안되는 것 같았다. 
다른 방법으로 구현해서 이부분은 자세히 보지 않았다. )

`getCurrentInstance()`[^fn3]라는 함수가 공식문서에서 라이브러리에서 advanced use case에서만 사용하고, app code단에서는 쓰지 말라고 대문짝만하게 경고한다. 
찝찝해서 다른 방법을 찾았다. 위에 꺼내오는 코드만 봐도 험난하고 쓰지 못하게 하려는 의지가 보인다.

### globalProperties 말고 다른 방법은 없나요? provide, inject 배달요
결론적으로 나는 `provide, inject`방식으로 구현했다. 그리고 provide하는 부분을 plugin으로 만들었다



```javascript
// api.js
import { App, inject } from "vue";

const ApiSymbol = Symbol("api");

function injectApi() {
  return inject(ApiSymbol);
}

export default function install(app) {
  app.provide(ApiSymbol, http);
}

export { injectApi };


```

사용하는 코드 
```javascript

//main.js 
import { createApp } from "vue";
import App from "./App.vue";
import api from "./api";

const app = createApp(App);
app.use(api);
app.mount("#app");


// component.vue
<script setup>
import { injectApi } from "@/api";

const api = injectApi(); 
api.get("/api/v1/users").then((res) => {
  console.log(res);
});
</script>

```

# 결론 
Vue3에서 Options API 사용하는 것이 아니라면, Composition API에서는 `provide, inject`형태를 추천한다.


# References
[^fn1]: [vue2: prototype](https://v2.vuejs.org/v2/cookbook/adding-instance-properties){:target="_blank"}
[^fn2]: [vue3: app.config.globalproperties](https://vuejs.org/api/application.html#app-config-globalproperties){:target="_blank"}
[^fn3]: [vue3: composition api - getcurrentinstance](https://v3.ru.vuejs.org/api/composition-api.html#getcurrentinstance){:target="_blank"}
[^fn4]: [stackoverflow: global var in vue3](https://stackoverflow.com/questions/65184107/how-to-use-vue-prototype-or-global-variable-in-vue-3){:target="_blank"}
[^fn5]: [blog: kyounghwan01](https://kyounghwan01.github.io/blog/Vue/vue3/global-state/#vue2%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5-global-%E1%84%87%E1%85%A7%E1%86%AB%E1%84%89%E1%85%AE-%E1%84%83%E1%85%A1%E1%84%85%E1%85%AE%E1%84%80%E1%85%B5){:target="_blank"}


