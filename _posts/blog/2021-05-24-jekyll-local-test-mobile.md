---
title: '로컬 제킬 깃헙 블로그 모바일에서 확인하는 법'
last_modified_at: 2021-05-24T01:24
categories:
  - Blog
tags:
  - minimal_mistakes
  - jekyll
toc: true
toc_sticky: true
---

## Summary 
제킬 깃헙 블로그를 작성/개발할 때 로컬 컴퓨터나 모바일 기계로 바로 확인하는 법을 알아봅니다. 이외의 경우들(라이브리로드, 미래 포스트)에 대해서도 알아봅니다.

## 제킬 프로젝트 로컬에서 실행하기 
아래 커맨드를 사용합니다.
```
bundle exec jekyll serve
```

간혹 위 커맨드가 아닌 아래 커맨드를 쓰인 글들을 발견할 수 있습니다.
```
jekyll serve
```
`bundle exec`을 넣어준 커맨드를 [사용하길 추천해 드립니다.](https://stackoverflow.com/questions/51157446/whats-the-difference-between-bundle-exec-jekyll-serve-and-jekyll-serve)
`bundle exec jekyll serve` 는 프로젝트 Gemfile/Gemfile.lock 에 지시된 제킬 서버 버전으로 실행시킵니다. 반면에 `jekyll serve` 는 어떤 제킬 서버의 어떤 버전을 실행시킨다고 합니다(글로벌한 gem 버전으로). 그 때문에 프로젝트가 원하는 버전이 실행 안 되어서 crash가 나거나 정확하지않은 결과물이 나올 수 있습니다. 이 깃헙블로그가 유일한 gem 프로젝트이신 분들은 차이점이 없을 것으로 생각합니다.
- [제킬 커뮤니티에서](https://talk.jekyllrb.com/t/jekyll-build-vs-bundle-exec-jekyll-build/5503) chuckhoupt 이란 분은 강력하게 `bundle exec jekyll serve`를 사용하라고 주장합니다. 



## 로컬에서 live-reload로 바로 확인하기 

```
bundle exec jekyll serve --livereload
```
로컬에서 파일 변경 시 자동으로 변경사항을 반영하도록 라이브리로드하는 옵션입니다.

참고로, `_config.yml`에서 변경한 것은 반영이 안 됩니다. 아마 처음 시작할 때 읽고 다시 읽지 않는 것 같습니다. 이런 경우, exit 하시고 다시 커맨드를 실행시켜주셔야 합니다. 


## 모바일 기계에서 확인하기
로컬 컴퓨터에서만 확인 말고, 폰으로도 확인이 필요할 때가 있습니다. 

1. 폰과 제킬을 돌릴 컴퓨터 둘 다 같은 네트워크를 가져야 합니다 (같은 와이파이에 접속). 
2. `bundle exec`에 `--host=0.0.0.0` 옵션을 넣어줍니다.
    ```
    bundle exec jekyll serve --host=0.0.0.0
    ```
  추가 호스트 옵션 없이 돌리면 [모바일에서 접속이 불가합니다](https://stackoverflow.com/questions/16608466/connect-to-a-locally-built-jekyll-server-using-mobile-devices-in-the-lan). 
  - 옵션 없이 돌리게 되면 loopback interface (127.0.0.1) 로컬호스트에 listen을 해서 그렇다고 합니다. `--host=0.0.0.0` 옵션을 주면, 모든 interface(0.0.0.0)에서 듣게 되므로, 모바일에서 로컬서버(컴퓨터) 아이피로 접속이 가능해집니다. 
  - (사담: 다른기계의 loopback interface는 접속이 불가한것인지 검색해보다가 알게되었습니다. [가능하다고 합니다.](https://stackoverflow.com/questions/45597362/can-devices-connect-to-loopback-address-of-another-device#:~:text=2%20Answers&text=Other%20devices%20can%20connect%20to,configure%20other%20addresses%20as%20well.) 그 다른 기계가 loopback 주소로 route만 있으면 가능하다고 합니다. 매뉴얼 static route 나 라우팅 프로토콜을 사용해서 가능하다고 합니다.)

3. 모바일 브라우저에서 `http://{컴퓨터 와이파이 아이피 주소(IPv4)}:{제킬 서버 포트}`로 접속해 주세요. 
  - 컴퓨터 아이피는 맥인 경우, 터미널에서 `ifconfig` 
  윈도우인 경우, `ipconfig`를 입력해서 볼 수 있습니다. 
  - 저는 제킬 기본 포트 4000을 사용해서 `http://172.30.1.41:4000/`으로 접속했습니다 


<img src='{{"/assets/images/posts/20210524_jekyll_mobile.png"| relative_url}}' alt='WIS 2021' style="width: 50%;" class="align-center">


혹시 똑같이 따라 해도 모바일 기계에서 접속이 안 된다면 컴퓨터 방화벽에서 제킬이 사용하고 있는 포트(4000)로 inbound 접속을 허용하는지 확인해보세요. 이 외에도 보안 관련 설정 때문에 안될 수도 있습니다.
{: .notice--info}


## 날짜가 미래인 포스트가 보이게 하기

로컬에서 포스트를 현재 시스템보다 미래의 날짜로 작성하면, 다음과 같은 터미널 메세지를 보여주면서 해당 글이 로컬에서 보이지 않습니다: `Skipping: _posts/2022-05-24-future-post.md has a future date`

이럴 때는, `--future` 옵션을 주면 해결됩니다. 

```
bundle exec jekyll serve --livereload --future
```



## 참고
제가 현재 로컬에서 쓰고 있는 커맨드입니다: 
```
bundle exec jekyll serve --livereload --future --host=0.0.0.0
```
