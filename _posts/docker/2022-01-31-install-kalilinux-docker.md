---
title: '칼리 리눅스 도커 컨테이너 설치와 VNC 설정'
last_modified_at: 2022-02-02T17:40
categories:
  - Docker
tags:
  - docker-compose
  - kalilinux 
  - vnc
toc: true
---


## 준비물
도커와 도커 컴포즈를 설치해 주세요. 아직 설치가 안 되셨다면 
[이 글]({{ site.url }}{{ site.baseurl }}/docker/docker-compose-install/){:target="_blank"}을 참고하세요. 




## 도커컴포즈 파일 및 도커컨테이너 생성
칼리 리눅스 컨테이너를 생성할 때 사용할 도커 컴포즈 파일을 만듭니다. 
{% highlight shell %}
vim kali.yml
{% endhighlight %}

{% highlight yaml %}
version: "3.9"

services:
  kali:
    image: kalilinux/kali-rolling
    container_name: 'kali'
    tty: true
    ports:
     - "25900:5900"
     - "25901:5901"
{% endhighlight %}


해당 kali.yml 파일을 저장합니다. 
이 파일을 사용해서 칼리리눅스 도커 컨테이너를 설치 및 실행시킵니다.
{% highlight shell %}
docker-compose -f kali.yml up -d

# 실행되고 있는지 확인
docker ps
{% endhighlight %}


`kali`라는 도커 컨테이너가 실행되고 있는지 확인합니다. 


## 도커 컨테이너 셋업
이제 이 칼리리눅스 컨테이너에 사용할 패키지를 설치해 봅니다.

{% highlight shell %}
# 컨테이너에 쉘을 실행시킵니다.
docker exec -it kali /bin/bash

apt update 
apt upgrade
{% endhighlight %}


### pentool 설치
`kalilinux/kali-rolling`는 필요한 리눅스 컴포넌트만 들고 있기 때문에 칼리 penetration test 툴을 따로 설치해줘야 합니다.[^fn1] 여러 패키지를 묶어놓은 메타패키지가 여러 개 있습니다. 저는 [`kali-tools-top10`](https://www.kali.org/tools/kali-meta/#kali-tools-top10){:target="_blank"}을 설치해줬습니다. 제일 중요한 10가지 앱을 설치한다고 합니다. 

```
apt install kali-tools-top10
```


### Desktop Environment 설치
VNC 접속할 때에 필요한 GUI를 위해 데스크탑 환경을 설치해줍니다. 데스크탑 환경도 여러 옵션이 있는데 저는 `kali-desktop-xfce`를 설치했습니다. 기본 데스크탑 환경에다가 사이즈가 작다고 합니다.

```
apt install kali-desktop-xfce
```

### VNC 서버 설치 
vnc접속을 위한 vnc서버를 설치합니다. 저는 `tightvncserver`를 설치했습니다. 
```
apt install tightvncserver
```

vnc 비밀번호를 세팅해줍니다.
```
vncpasswd
```





환경변수를 세팅해주고 vnc서버를 실행시켜줍니다.
{% highlight shell %}
export USER=root 
tightvncserver :0 -geometry 1280x800 -depth 16 -pixelformat rgb565 
# 다음과 같은 라인이 프린트됩니다.
# New 'X' desktop is aa18211a10aa:0
# Starting applications specified in /root/.vnc/xstartup
# Log file is /root/.vnc/aa18211a10aa:0.log
{% endhighlight %}


## VNC 접속 
이제 칼리 리눅스 컨테이너로 vnc접속을 해보면 되겠죠? 

저는 맥을 이용했기 때문에 `Screen Sharing`을 이용해서 접속했습니다. 가지고 계신 VNC client을 이용하시면 될 것 같습니다. 주소는 `127.0.0.1:25900`이고 비밀번호는 위에서 세팅해준 번호를 넣어줬습니다. 

![Screen Sharing 화면]({{"/assets/images/posts/20220131_vnc_sharescreen.png"| relative_url}})
짠! 잘 보이죠?


### (옵션) novnc 설치
브라우저를 통해서 VNC접속을 해봅니다. 컨테이너 안에서 다음을 실행해주세요. 

{% highlight shell %}
apt install net-tools 
apt install novnc

# vnc서버 포트 5900에서 웹소켓 서버 포트 5901로 터널링 해줍니다.
/usr/share/novnc/utils/launch.sh --listen 5901 --vnc localhost:5900
{% endhighlight %}


![novnc 실행 화면]({{"/assets/images/posts/20220131_novnc_1.png"| relative_url}})

이제 브라우저에서 `http://127.0.0.1:25901/vnc.html`로 접속하면 다음과 같은 화면이 뜹니다.
![novnc 화면]({{"/assets/images/posts/20220131_novnc_2.png"| relative_url}})

`Connect`를 클릭해서 세팅해준 vnc 비밀번호를 입력하면 칼리리눅스 화면을 볼 수 있습니다.

![novnc 화면]({{"/assets/images/posts/20220131_novnc_3.png"| relative_url}})

## Troubleshooting
### apt update시 에러
제가 이 글을 작성할 당시 첫날에는 다운받아지다가 그다음 날부터 도커 컨테이너 생성 후 `apt update`를 하면 다음과 같은 에러가 발생했습니다. 

![칼리 미러사이트 에러 화면]({{"/assets/images/posts/20220202_kalimirror_1.png"| relative_url}})

해당 미러 사이트에 접속했는데 파일들도 제대로 있는 것 같았는데 Release 폴더를 클릭하니 404 not found 에러가 떴었습니다. 구글링해 보니 `/etc/apt/sources.list`를 수정해서 해결하신 분들이 있었습니다. 수정해서 넣어봤지만, 에러가 사라지지 않았습니다. 그리고 [칼리리눅스 오피셜 사이트](https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/){:target="_blank"}에서 사용하라는 `/etc/apt/sources.list`가 기존 것과 동일했습니다. 해당 미러사이트를 확인해보니 에러가 발생한 날에 서버 파일 업데이트가 있었습니다. 아마 서버 업데이트 하면서 파일이 제대로 안 올라간 오류가 있는 것 같습니다. 해당 서버 위치가 한국이란 걸 보고, 다른 나라에 있는 서버를 사용할 수 있도록 미국 vpn으로 우회했습니다(저는 그냥 맥 앱스토어에 vpn검색해서 VPN Plus라는 앱을 사용했습니다). 그러고 나서 다시 `apt update`했더니 빰!! 

![칼리 미러사이트 working 화면]({{"/assets/images/posts/20220202_kalimirror_2.png"| relative_url}})

드디어 됐습니다. 흑흑 미국서버야 넌 멀쩡했구나🥲 설치가 잘못된 줄 알고 삽질했던 시간이여 안녕!!!!!




미래에는 수정될 것 같지만 저처럼 안되시는 분은 저와 같은 방법으로 당장 해결할 수 있을 것 같습니다. 


### vnc 화면이 회색 및 접속 불가
vnc client로 접속했는데 화면이 회색으로 뜨지 않나요? 해당 vnc 연결을 끊고 다음을 확인해 주세요.
  1. 데스크탑 환경이 제대로 설치되어있는지 확인해 주세요. 
  2. 컨테이너 안에서 실행되는 vnc server를 다시 시작해주세요.

컨테이너 안에서 돌고 있는 server를 찾아서 끄는 방법입니다.
{% highlight shell %}
ps ax | grep vnc 
# 몇 번에서 실행되고 있는지 확인해주세요. 저 같은 경우는 :0 이였습니다. 
# 13026 pts/1    S      0:00 Xtightvnc :0 -desktop X -auth /root/.....
# 해당 번호를 이용해서 kill 해주세요.
vncserver -kill :0
# Killing Xtightvnc process ID 13026

# vnc server를 다시 시작해주세요. 
tightvncserver :0 -geometry 1280x800 -depth 16 -pixelformat rgb565
{% endhighlight %}





## References
[^fn1]: [dog2's blog](https://dog.wtf/tech/run-kali-in-docker-and-install-desktop-environment-and-vnc/){:target="_blank"}