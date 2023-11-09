---
title: 'Storage: RAID & LUN'
last_modified_at: 2023-11-09T23:42
categories:
  - storage
tags:
  - storage
toc: true
toc_sticky: true
---


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

# SideNote
## Stripe
- 여러개의 디스크에 데이터가 분산되어 저장되는 것


# References
[^fn1]: [A Korean in Beijing](https://koreaninbeijing.tistory.com/417){:target="_blank"}
[^fn2]: [1nfra IT Blog: LUN(Logical Unit Number)이란](https://blog.1nfra.kr/226){:target="_blank"}
