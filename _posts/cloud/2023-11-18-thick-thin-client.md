---
title: 'Thick vs. Thin vs. Zero Clients'
last_modified_at: 2023-11-19T20:50
categories:
  - cloud
tags:
  - architecture
  - vdi
toc: true
toc_sticky: true
---

# 개요
Thick 또는 Thin client라는 단어는 네트워크로 이어진 클라이언트-서버 아키텍처에서 시스템/장치가 어떻게 데이터를 프로세싱하고 어느정도의 자원을 요구하는지에 따라 분류할 때 쓰인다. 각 단어의 뜻을 알아보자. 

# Thick Client
- "heavy", "fat", 또는 "rich" client 라고도 부름 
- client 쪽에서 서버쪽 리소스를 덜 의존하고, 데이터 프로세싱을 직접함
- 대부분의 앱 로직이나 프로세싱이 일어남 (local에서 프로세싱)
- thin client보다 보통 더 좋은 자원(CPU, 메모리 등)을 요구하고 비용이 더 많이 듬.
- 하드 드라이브와 미디어 포트를 가지고 있어 thin client보다 안정성이 떨어짐.[^fn4]
- 네트워크로 서버에 연결될 수 있지만, 꼭 연결될 필요없음. 가끔 연결 필요 (앱 다운받거나, remote db데이터 업데이트하거나 등)
- 자신이 대부분의 필요한 자원을 가지고 있기 때문에 독립적으로 동작 가능. 
- 백업 서버/앱이 존재함. 네트워크 연결끊어져도 ms office 온라인 사이트 접속못해도 로컬에 설치한 ms word로 워드 작성 가능.
- 예) 전통적인 완전한 PC. 자신의 고유 하드 드라이브, s/w 앱을 가지고있음 [^fn1]

# Thin Client 
- lean client 라고도 부름
- thick client의 반대
- 서버쪽 프로세싱에 의존하고 최소한의 리소스를 가짐 
- 가벼운 인터페이스를 가지고, 서버/클라우드쪽에 대부분의 데이터 프로세싱을 위임 (자기 자신 로컬 프로세싱 별로안함)
- 컴퓨팅/프로세싱하기 할때 네트워크 연결에 의존함. 지속적으로 센터(central) 서버랑 연결 필요.
- thick client보다 보안 위협이 적음[^fn3]
- 장치 예) 스마트폰, 태블릿
- 예) VDI, virtual desktops, virtualized apps [^fn2]
  - virtual desktop이 데이터 센터에서 호스팅되면, thin client는 백엔드 서버의 터미널 역할


# Zero Client 
- "ultra thin" client [^fn5]
- Thin client보다 더 얇고, 비용 효율적 [^fn4]
- thin client처럼 central서버나 퍼블릭 클라우드에 연결해서 데이터와 앱에 접근
- 저장장치, OS 조차 없음 
  - thin client 는 보통 os와, 플래시 메모리에 설정(configuration settings)을 저장하는데, zero client는 이런것도 저장하지 않음 [^fn6]
- configuration필요없고, 거의 s/w도 필요없음
- 사용자한테 데이터 전달/앱 딜리버리하는 터미널처럼만 작동
- thick, thin 보다 제일 보안 위협 적음
- thick, thin보다 제일 쌈

# References
[^fn1]: [techtarget: thick client (fat client)](https://www.techtarget.com/whatis/definition/fat-client-thick-client){:target="_blank"}
[^fn2]: [techtarget: thin client](https://www.techtarget.com/searchnetworking/definition/thin-client){:target="_blank"}
[^fn3]: [geeksforgeeks: difference between thin clients and thick clients](https://www.geeksforgeeks.org/difference-between-thin-clients-and-thick-clients/){:target="_blank"}

[^fn4]: [VDI 하드웨어 비교: Thin vs. thick vs. zero clients](https://m.blog.naver.com/ki630808/222133521770){:target="_blank"}
[^fn5]: [Thick vs. Thin vs. Zero Clients: Which Model Is Right for You?](https://www.cdw.com/content/cdw/en/articles/hardware/thick-vs-thin-vs-zero-clients.html){:target="_blank"}
[^fn6]: [VDI hardware comparison: Thin vs. thick vs. zero clients](https://www.techtarget.com/searchvirtualdesktop/feature/VDI-hardware-comparison-Thin-vs-thick-vs-zero-clients){:target="_blank"}
