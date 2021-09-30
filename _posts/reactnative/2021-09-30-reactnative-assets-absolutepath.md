---
title: '절대 경로로 정적파일(이미지) 가져오기'
last_modified_at: 2021-09-30T19:10
categories:
  - ReactNative
tags:
  - reactnative
  - npm
  - absolute path
  - assets
toc: true
  
---

리액트 네이티브 앱에서 절대 경로로 깔끔하게 정적파일 가져오는 법을 소개합니다.🥳 \
Using absolute paths to access static assets in React Native apps

## 왜 사서 고생하나요? 
왜라고 물으신다면 찝찝했습니다. \
리액트 네이티브 앱 개발을 하다가 로컬 이미지 주소를 가져와야 하는 부분이 생겼습니다. 
처음 한두 개는 어떻게 하고 있었는데, 거슬렸습니다. 네..! 

저 같은 경우는 소스코드를 src 폴더 안에 components, screens 등 여러 폴더에 작성했는데 
상대 경로로 하나하나 가져오려다 보니깐 나중에 제 컴포넌트나 정적파일 assets들 위치 바뀌면 관리하기 어려울 것 같다고 판단했습니다. 
물론 지금 보니깐 그렇게 많이 가져오지 않는데 괜히 오바한 건가 싶기도 합니다. 히히 ^^

이 더티하고 긴 경로 말고 신박하고 간단하게 가져올 방법이 없을까 하다가 찾았습니다! [강 같은 글](https://www.willowtreeapps.com/craft/react-native-tips-and-tricks-2-0-managing-static-assets-with-absolute-paths)[^fn1] 🥺

npm 모듈이랑 패키지 관련 지식이 부족해서 조금 해맸지만 해결했습니다. 

## 어떻게?
정적 파일을 관리하는 폴더(`assets`) 안에 js파일과 package.json을 추가합니다. \
js file안 에 우리가 쓸 이미지 주소(reference)를 dictionary object 안에 작성하고 export 해서 모듈화해줍니다!

이제 이 export된 dictionary object를 앱 다른 곳에서도 접근 가능할 수 있도록 
package.json을 이용해서 해당 폴더를 패키지화하면 됩니다.

### 코드 

현재 제 폴더 구조는 대충 다음과 같습니다. \
(저랑 다르게 관리하시는 분들 팁 좀 주세요..! 전 따라쟁이라 미디엄 글 찾아서 따라했습니다. assets 폴더를 src 안으로 넣을까 고민중입니다)
```
app
├── App.js
├── assets
│   ├── images.js
│   ├── img
│   │   ├── a.png
│   │   ├── b.png
│   │   └── c.png
│   └── package.json
├── package.json
└── src
    ├── components
    │   └── ...
    ├── screens
    │   ├── ...
    │   └── AScreen.js
    └── ...
```

`AScreen.js` 안에서 `assets/img/`안에 있는 파일들 가져오는데 귀찮더라고요. 네! 제가 귀차니즘, 귀차니즘이 접니다. 


`assets`폴더안에 `images.js`와 `package.json`를 추가했습니다. 

`assets/images.js`:
```js
const images = {
    AIcon: require('./img/a.png'),
    BIcon: require('./img/b.png'),
    CIcon: require('./img/c.png'),
}

export default images;
```



`assets/package.json`:
```json
{
    "name": "assets",
    "version": "1.0.0",
    "description": "",
    "main": "images.js",
    "author": "Minjeong Choi",
    "license": "ISC"
  }
```
저는 `npm init`을 사용해서 쓸데없는 게 있는데 `name`과 `main`만 있으면 되는 것 같습니다. 



이제 가져오는 부분을 보여드리겠습니다: \
`src/screens/AScreen.js`
```js
import React from 'react';
import { Image } from 'react-native';

import Images from 'assets'   /////// 이렇게 추가해주세요


const AScreen = () => {
    ...
    return(
        ...
        {/* Images.AIcon 이렇게 가져오면 끝 */}
        <Image source={Images.AIcon} /> 
        ...
    );
};

export default AScreen;

```




## 헤맨 부분 
`npm install`만 해왔던 저는 위에 찾은 글 따라 했는데 에러가 뜨더라고요. 아니 선생님! `name`만 넣으면 된다면서요! 🥺🥺

node module과 package 개념이 제대로 없어서 생겼던 문제였습니다. 관련해서 포스트로 정리할 예정입니다. 
module을 package화하려면 조건이 있다고합니다. [^fn3] \
우선 `package.json` 이 있어야 합니다. Node.js 의 require() 펑션으로 가져올 수 있게 하려면 `package.json`안에 `main` 필드가 필수로 있어야 하는데, `main` 부분이 없어서 가져올 때 계속 에러가 생겼었습니다.





## 후기 
우선 한 곳에서만 정적파일 위치 스트링 관리하면 돼서 편하긴 합니다. 
갑자기 궁금해진 게 image.js안에 있는 images 오브젝트가 어마어마하게 길어지면 어떻게 될까요? 성능에 문제가 될까요? 상관없을까요? 제 앱 같은 경우는 assets 가 어마어마하게 많을 것 같지는 않지만, 이미지, 비디오 등 타입별로 나누거나 컴포넌트 주제별로 나눠서 export 해도 될 것 같습니다. 


위 방법 말고도 여러 방법이 있는 것 같습니다.[^fn2]
- `file:../`을 이용한 local dependency를 사용할 수도 있을 것 같습니다. 
- `npm link`같은 경우, 폴더를 패키지처럼 '설치'해준다고 합니다. 심링크를 만들어서 차후에 패키지 내용이 바뀌어도 자동으로 최신 버전으로 반영이 된다고 합니다. 근데 어떤 글에선 react native 에서는 [적용이 안 된다고](https://stackoverflow.com/questions/55953564/how-to-use-symlinks-in-react-native-projet) 합니다. 훔훔
- TS를 쓰면 [tsconfig.json 에 path들을 지정](https://stackoverflow.com/questions/35068813/typescript-how-to-not-use-relative-paths-to-import-classes/49757060)
할 수 있다고 합니다. 나중에 요긴하게 쓸 것 같습니다. 





지적 환영합니다. 알려주세요!




## 참고자료
[^fn1]: [WillowTree 블로그](https://www.willowtreeapps.com/craft/react-native-tips-and-tricks-2-0-managing-static-assets-with-absolute-paths)
[^fn2]: [sanggeun choi님 블로그](https://sg-choi.tistory.com/396)
[^fn3]: [stackoverflow]([https://stackoverflow.com/questions/20008442/difference-between-a-module-and-a-package-in-node-js](https://stackoverflow.com/questions/20008442/difference-between-a-module-and-a-package-in-node-js))