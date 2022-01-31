---
title: '짱쉬운 도커와 도커컴포즈 설치'
last_modified_at: 2022-01-31T21:54
categories:
  - docker
tags:
  - docker-compose
  - install
toc: true
---

맥OS 기준으로 쓰여진 글입니다.
{: .notice}


## 도커 설치
너도나도 쓰는 도커세상. 얼른 설치해보자.

저는 맥에서 설치했기 때문에 도커 공식 사이트에서 [Docker Desktop on Mac](https://docs.docker.com/desktop/mac/install/){:target="_blank"}을 클릭해서 설치했습니다. 

다른 운영체제를 사용하시는 분들은 이 [페이지](https://docs.docker.com/engine/install/){:target="_blank"}를 참고하셔서 따라하시면 됩니다. 전부터 느꼈지만 도커 공식문서가 굉장히 디테일하고 친절한 것 같습니다. 



## 도커 컴포즈 설치 
도커 컴포즈는 선택사항인데 저는 도커 컨테이너를 yaml파일로 생성하거나 관리할 때 매우 편했습니다. 다운 받으시길 추천합니다! 


도커 컴포즈도 짱짱 간단합니다. 
저는 오늘 기준 최신버전인 `2.2.3`버전을 설치하겠습니다.

```
sudo curl -L "https://github.com/docker/compose/releases/download/2.2.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

마지막 코맨드까지 날리면 어떤 버전의 도커컴포즈가 설치되어있는지 뜹니다. 

도커 컴포즈도 새로운 버전이 나오니 최신 버전을 설치하고 싶으시다면 [공식 release 페이지](https://github.com/docker/compose/releases/){:target="_blank"}에서 버전을 체크해서 설치 코맨드에서 숫자부분을 해당 버전으로 바꿔주시면 됩니다!
(예: 버전 5.0.0. 이라면 `sudo curl -L "https://github.com/docker/compose/releases/download/5.0.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose` 으로 바꿔주세요)


끝!🥰

