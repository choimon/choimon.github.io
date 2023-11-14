---
title: 'Storage: RAID, LUN, SAN'
last_modified_at: 2023-11-14T22:51
categories:
  - storage
tags:
  - storage
  - RAID
  - LUN
toc: true
toc_sticky: true
---

# 개요
회사에서 물리/가상자원 스토리지 모니터링 업무를 설계해야하는데 모르는 용어가 많아서 정리해봤다. <br/>

[//]: # (물리 스토리지 장비부터 이 장비를 연결한 cloud platform에서 할당한 가상 storage 자원까지 모니터링을 해야하는데 물리부터 막혔다. )


# RAID(Redundant Array of Inexpensive/Independent Disks)
- 여러 개 하드 디스크를 묶어 "하나의 디스크"처럼 사용하는 것 
- (+)장점[^fn1]
  - 빠른 속도: 서버에서 데이터를 읽을 때, 여러 개 디스크에서 데이터를 읽을 수 있음. 더 좋은 성능
  - 안정성: RAID 컨트롤러가 손실된 데이터를 복구할 수 있다고 함
- RAID엔  0,1,2,3,4,5,6등 여러 종류가 있음

## RAID 0
- 패리티 없는 디스크 묶음 (Disk Striping without parity)
- 데이터를 여러 개 디스크에 분산 저장
- 패리티 없음 -> 안정성 떨어짐, 저장용량 제일 큼
- 예) 데이터 123456789가 여러개의 디스크에 분산 저장됨. disk A,B,C가 있다면, A에 데이터 1,2,3, B에 데이터 4,5,6, C에 데이터 7,8,9 저장

## RAID1
- 디스크 미러링 (Disk Mirroring)
- 디스크 1개에 장애 발생해도, 다른 디스크의 데이터로 복구 가능 
- 예) disk A,B가 있다면, A에 1,2,3,4,5,6,7,8,9, B에 1,2,3,4,5,6,7,8,9

## RAID1+0
- RAID 1과 RAID 0을 합친 것
- Mirrored Stripes 또는 Striped Mirrors라고도 함
- 예) disk A,B,C,D가 있다면, A,B에 데이터 나누어 저장하고, C,D에 미러링. 
  A에 1,2,3,4,5와 B에 6,7,8,9, 이렇게 한 묶음. 그리고, C에 1,2,3,4,5와 D에 6,7,8,9

## RAID2
- 더 이상 사용하지 않음

## RAID3
- parity만 저장하는 디스크 존재.
- 다른 디스크는 striping 되어 사용
- bit-level stripe

## RAID4
- RAID3과 비슷하지만, block 단위로 striping

## RAID5
- Disk striping with parity
- 모든 디스크를 stripe하면서 parity도 같이 기억함 (데이터복구를 위한 parity)
- 성능 & 안정성 균형 잡힘
- 가장 많이 사용됨
- 예) 디스크 A,B,C가 있다면, A에 1,2,3과 parity 7,8,9. B에 4,5,6과 parity 1,2,3. C에 7,8,9와 parity 4,5,6

## RAID6
- 2개의 parity 디스크 사용
- 가장 안정성 높음
- 오랜시간 데이터 보관할때 적합
- 성능은 떨어짐
- 예) 디스크 A,B,C,D가 있다면, A에 1,2,3,4,5, B에 6,7,8,9 C에 parity 1,2,3,4,5,6,7,8,9, D에 parity 1,2,3,4,5,6,7,8,9

# LUN (Logical Unit Number)
- RAID의 스토리지 공간들을 나타냄 
- RAID의 모든 공간 또는 공간의 일부(partition)
- "서버에서 한 개의 스토리지 공간으로 인식하는 단위"라고 함 [^fn1]
- 예) LUN0을 서버0에 할당함. LUN1을 서버1에 할당함. 


# SideNote
## Stripe
- 여러개의 디스크에 데이터가 분산되어 저장되는 것


## SAN (Storage Area Network)
- 스토리지 리소스를 여러 서버에 연결해주는 전용 고속 네트워크
- block단위로 스토리지 디바이스에 접근
- 보통 Fibre Channel or iSCSI protocols 사용
- RAID 구성하고, 서버한테 LUN 할당해줄 때 SAN을 사용함
- (+) 빠른 속도, 안정적
- (-) 비싼 초기 구축 비용, 복잡한 구성


## NAS (Network Attached Storage)
- 네트워크로 통신해서 저장장치에 연결해줌
- TCP/IP와 같은 표준 네트워크로 Network Switch와 연결하여 사용 
- 파일 공유할 때 많이 사용 
- (-) 네트워크 환경에 따라 트래픽 문제 발생 가능

## 데이터 전송 (프로토콜, 표준) 

### SCSI (Samll Computer System Interface)
- '스카시'
- 컴퓨터와 주변장치 사이의 데이터 전송을 위한 인터페이스 표준
- 직렬 방식으로 연결
- SCSI케이블 같이 케이블로 보통 연결됨


### SAS(Serial Attached SCSI)
- 데이터 스토리지 시설과 송수신할 수 있는 직렬 프로토콜 [^fn5]이라고 함
- direct connections bt servers and storage devices
- 기존 SCSI보다 더 빠른 속도, 더 긴 케이블 길이, 더 많은 디바이스 연결 가능

### FC(Fibre Channel)
- '에프시', '파이버채널'
- 서버를 스토리지 장비에 연결할 때 사용되는 고속, 고성능 네트워크 기술
- FC 케이블과 스위치에서 동작하는 네트워크 프로토콜
- gigabit 이상 속도, low latency, high bandwidth 데이터 전송 가능
- TCP/IP보다 구조가 단순
- SAN환경에서 iSCSI와 함께 블록 데이터 전송할 때 일반적으로 사용됨[^fn6]


### iSCSI (Internet Small Computer System Interface)
- IP 네트워크 위에서(network layer) block단위 스토리지 액세스할때 사용되는 protocol
- IP 패킷에 read, write과 같은 SCSI 명령을 감싸서(encapsulate) 전송
- 서버랑 스토리지 디바이스가 IP 네트워크 위에서 통신하면서 데이터 전송할 수 있게함
- 마치 스토리지가 서버에 직접 연결된 것처럼 사용할 수 있음
- FC사용하지 않고도 기존 이더넷 스위치로 SAN환경 구축할 수 있어서 비용절감
- FC에 비해 안정성 성능은 떨어짐 (TCP/IP를 통해 SCSI패킷 전송함)

[//]: # (## cloud platform 에서 물리 스토리지 장비 사용하기)

[//]: # (- OpenStack 이라는 클라우드 플랫폼을 예제로 들자면, OpenStack에서는 스토리지 관련 Cinder와 Manila 스토리지 서비스가 존재함)

[//]: # (- 내가 NetApp이라는 물리장비를 사용해 OpenStack 스토리지 가상자원을 만들고 싶다면, Cinder쪽에 명령어로 NetApp을 연결해야함)

[//]: # (- 이 연결한 NetApp을 이제 Cinder 쪽 volume으로 사용할 수 있다고 함. vm instance를 생성하고, Cinder volume을 연결하고 싶을 때 volume type에 뜨는 형태라고 이해)

[//]: # (- Cinder API를 사용해 volume을 생성하겠지만, 실질적으로 사용하는 물리 장비는 NetApp일거임. )

# References
[^fn1]: [A Korean in Beijing](https://koreaninbeijing.tistory.com/417){:target="_blank"}
[^fn2]: [1nfra IT Blog: LUN(Logical Unit Number)이란](https://blog.1nfra.kr/226){:target="_blank"}
[^fn3]: [NetApp: Volumes, qtrees, files, and LUNs](https://docs.netapp.com/us-en/ontap/concepts/volumes-qtrees-files-luns-concept.html){:target="_blank"}
[^fn4]: [동국시스템스: Volumes, qtrees, files, and LUNs](https://www.dknyou.com/blog/?q=YToxOntzOjEyOiJrZXl3b3JkX3R5cGUiO3M6MzoiYWxsIjt9&bmode=view&idx=6123731&t=board){:target="_blank"}
[^fn5]: [저장소 프로토콜 SCSI, iSCSI, SAS, FC 구분과 용도에 대해서](https://blog.naver.com/zagatoson/222446702817)
[^fn6]: [스토리지 프로토콜](https://tech.gluesys.com/blog/2019/12/17/storage_2_intro.html)