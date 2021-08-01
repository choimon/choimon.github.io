---
title: '프로세스 동기화(Process Synchronization)'
last_modified_at: 2021-08-01T21:08
categories:
  - OS
tags:
  - process
  - thread
  - synchronization
toc: true
toc_sticky: true
---
# Introduction 

동기화 관련해서 프로세스를 2가지 타입으로 구분할 수 있다. 
- Cooperative process: 다른 프로세스의 수행에 영향을 받거나 주는 프로세스
- Independent process: 다른 프로세스의 수행에 아무런 영향을 미치지 않는 독립적인 프로세스 

컴퓨터의 메모리에 여러 프로세스가 존재한다.[^fn1] 프로세스가 하나의 공유 메모리나 또 다른 프로세스에 접근할 때에는 프로세스 동기화 문제가 생길 수 있으므로 조심해야 한다. 프로세스 동기화 문제는 프로세스가 Cooperative 프로세스일 경우에 발생한다.[^fn2] Cooperative 프로세스에서 자원이 공유되기 때문이다.  

프로세스 동기화는 여러 프로세스가 **공유하는 자원의 일관성을 유지하는 것**이다. \
예) 영화관 표 예매 시스템에서 예매하려는 사용자가 동시에 여러명이여도, 한 영화 좌석은 한 사람만이 예매할 수 있도록 해야한다. 

# 동기화 문제 

## Race Condition 
동시에 여러 개의 프로세스가 동일한 자료를 접근하여 경쟁하는 현상
- 여러 프로세스가 동일한 코드를 수행하거나 같은 메모리나 공유 변수 접근할 때 각자 자신의 결과값으로 앞다투어 '경쟁'하게 되어 결과 값이나 공유 변수의 값이 틀릴 수 있다. 
- 임계구역에서 나타날 수 있다. 임계구역에서 여러 스레드가 수행하게 되면서 결과값이 스레드 수행 순서에 따라 달라지기 때문이다. 
  - 임계구역의 race condition은 임계구역을 atomic instruction으로 처리하면 피할 수 있다. 
  - 적절한 스레드 동기화(lock, atomic 변수 이용)를 통해서도 race condition을 예방할 수 있다. 

### 공통변수에 대한 동시 업데이트 문제 
(concurrent updates on common variables)
- 은행 계좌 문제 (Bank Account Problem)
  - 하나의 공유 자원인 은행 계좌에 입금, 출금을 각각 다른 스레드로 실행 하여 시간 지연을 발생시킨 경우 비정상적인 계좌 잔액이 나온다.

  관련 코드는 추후 업데이트 예정입니다.
  {: .notice}
  
  - 해결하는 방법: 공통변수에 접근하는 스레드는 하나만 존재하도록 관리한다. 이러한 공통변수 구역을 임계구역(critical section)이라 한다. 

## 임계구역 문제 (Critical Section Problem)

임계구역은 여러 개의 스레드가 수행되는 시스템에서 스레드들이 공유하는 데이터(변수, 테이블, 파일)을 변경하는 코드 영역이다. 동기화에서 중요한 문제 중 하나다. 

```
do {
  // entry section: 프로레스/스레드가 임계구역으로 들어가기위해 요청한다.
    critical section 
  // exit section 
    remainder section
} while (TRUE);
```
이렇게 표현할 수도 있다: 

```
// arbitrary code (initialization )

EnterCriticalSection(cs); 

// The code that constitutes the CS.
// Only one process can be executing this code at the same time. 

LeaveCriticalSection(cs);

// Remainder section. Some arbitrary code. 

```
[^fn3]



```java
void deposit(int amount) {
  balance = balance + amount;
}
void withdraw(int amount) {
  balance = balance - amount;
}
```

위 코드는 은행계좌(공유하는 변수, balance)를 입금과 출금하는 코드며 은행계좌 문제의 임계구역이라고 할 수 있다. 

포스트에서 스레드라고 표현한 부분이 있는데, 이 부분은 프로세스일 때에도 해당된다.
{: .notice--info}

임계구역 문제를 해결하기 위해서는 3가지 조건이 필요하다.
- Mutual Exclusion(상호배타)
  - 오직 한 스레드/프로세스만이 진입 가능하다. 
  - 한 스레드/프로세스가 임계구역에서 수행중일 때에는 다른 스레드/프로세스는 이 구역에 접근/수행할 수 없다. 
- Progress(진행)
  - 한 임계구역에 접근하는 스레드를 결정하는 것은 유한 시간 이내에 이루어져야 한다. 
  - 어느 순간에는 스레드가 수행을 끝낼 수 있어야 한다. 
- Bounded waiting(유한대기)
  - 임계구역으로 진입하기 위해 대기하는 모든 스레드는 유한 시간 이내에 해당 임계구역으로 진입할 수 있어야 한다.




# References
[^fn1]:[codemcd님 velog - 프로세스 동기화](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-8.-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EB%8F%99%EA%B8%B0%ED%99%94-1){:target="_blank"}
[^fn2]:[geeksforgeeks](https://www.geeksforgeeks.org/introduction-of-process-synchronization/){:target="_blank"}
[^fn3]:[stackoverflow](https://stackoverflow.com/questions/33143779/what-is-progress-and-bounded-waiting-in-critical-section){:target="_blank"}






