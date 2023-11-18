---
title: 'Thick & Think Clients'
last_modified_at: 2023-11-18T21:35
categories:
  - cloud
tags:
  - architecture
toc: true
toc_sticky: true
---

# 개요
Thick 또는 Thin client라는 단어는 네트워크로 이어진 클라이언트-서버 아키텍처에서 시스템이 어떻게 데이터를 프로세싱하고 어느정도의 자원을 요구하는지에 따라 분류할 때 쓰인다. 각 단어의 뜻을 알아보자. 

# Thick Client
- heavy, fat, 또는 rich client 라고도 부름 
- client 쪽에서 서버쪽 리소스를 덜 의존하고, 데이터 프로세싱을 직접함
- 대부분의 앱 로직이나 프로세싱이 일어남 (local에서 프로세싱)
- thin client보다 보통 더 좋은 자원(CPU, 메모리 등)을 요구함 
- 네트워크로 서버에 연결될 수 있지만, 꼭 연결될 필요없음. 가끔 연결 필요 (앱 다운받거나, remote db데이터 업데이트하거나 등)
- 자신이 대부분의 필요한 자원을 가지고 있기 때문에 독립적으로 동작 가능. 
- 백업 서버/앱이 존재함. 네트워크 연결끊어져도 ms office 온라인 사이트 접속못해도 로컬에 설치한 ms word로 워드 작성 가능.
- 예1) 물리적 PC가 될 수 있음. 자신의 고유 하드 드라이브, s/w 앱을 가지고있음 [^fn1]
- 예2) 비디오 편집 소프트웨어(Adobe Premiere Pro), 사용자 컴퓨터에서 설치되고 실행되는 게임 앱

# Thin Client 
- lean client 라고도 부름
- thick client의 반대
- 서버쪽 프로세싱에 의존하고 최소한의 리소스를 가짐 
- 가벼운 인터페이스를 가지고, 서버/클라우드쪽에 대부분의 데이터 프로세싱을 위임 (자기 자신 로컬 프로세싱 별로안함)
- 컴퓨팅/프로세싱하기 할때 네트워크 연결에 의존함. 지속적으로 서버랑 연결 필요.
- thick client보다 보안 위협이 적다고함[^fn3]
- 예) VDI, virtual desktops, virtualized apps [^fn2]
- 예) web베이스 앱 (Google Docs, Salesforce)




# References
[^fn1]: [techtarget: thick client (fat client)](https://www.techtarget.com/whatis/definition/fat-client-thick-client){:target="_blank"}
[^fn2]: [techtarget: thin client](https://www.techtarget.com/searchnetworking/definition/thin-client){:target="_blank"}
[^fn3]: [geeksforgeeks: difference between thin clients and thick clients](https://www.geeksforgeeks.org/difference-between-thin-clients-and-thick-clients/){:target="_blank"}
