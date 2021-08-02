---
title: '프로세스 동기화(Process Synchronization)'
last_modified_at: 2021-08-02T23:10
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

프로세스가 동시에 실행될 때 두 가지 유형으로 구분할 수 있다. 
- 협력 프로세스(Cooperating process): 다른 프로세스의 수행에 영향을 받거나 주는 프로세스
- 독립적 프로세스(Independent process): 다른 프로세스의 수행에 아무런 영향을 미치지 않는 독립적인 프로세스 


컴퓨터의 메모리에 여러 프로세스가 존재한다.[^fn1] 프로세스가 하나의 공유 메모리나 또 다른 프로세스에 접근할 때에는 프로세스 동기화 문제(데이터 불일치 등)가 생길 수 있으므로 조심해야 한다. 프로세스 동기화 문제는 프로세스가 협력 프로세스일 경우에 발생한다.[^fn2] 협력 프로세스에서 자원이 공유되기 때문이다. 


공유 데이터의 동시 접근은 데이터의 불일치 문제를 발생시킬 수 있다. \
프로세스 동기화는 여러 프로세스가 **공유하는 자원의 일관성을 유지하는 것**이다. \
일관성 유지를 위해서 협력 프로세스간의 실행 순서를 정해주어야 한다.

프로세스 동기화는 병행 제어(concurrency control)라고도 부른다.[^fn4]

글에서 프로세스 대신에 스레드라고 표현한 부분이 있는데, 이 부분은 프로세스일 때에도 해당된다. 즉, 프로세스 또는 스레드라고 해석하면 된다.
{: .notice--info}



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



### Solution
하나의 프로세스가 임계구역에 있을 때 다른 모든 프로세스는 이 구역에 들어갈 수 없어야 한다. 

임계구역 문제를 해결하기 위해서는 3가지 조건이 필요하다.
- Mutual Exclusion(상호배타)
  - 오직 한 스레드/프로세스만이 진입 가능하다. 
  - 한 스레드/프로세스가 임계구역에서 수행중일 때에는 다른 스레드/프로세스는 이 구역에 접근/수행할 수 없다. 
- Progress(진행)
  - 한 임계구역에 접근하는 스레드를 결정하는 것은 유한 시간 이내에 이루어져야 한다. 
  - 어느 순간에는 스레드가 수행을 끝낼 수 있어야 한다. 
- Bounded waiting(유한대기)
  - 임계구역으로 진입하기 위해 대기하는 모든 스레드는 유한 시간 이내에 해당 임계구역으로 진입할 수 있어야 한다.


#### 프로세스/스레드 동기화의 목적 
- 원하는 결과값을 도출하도록 임계구역 문제를 해결한다. 
- 프로세스의 실행 순서를 원하는대로 제어한다. 
- 비효율성(Busy wait등)을 제거한다. 
  - Busy wait: while문 반복 대기를 통해 지속적으로 CPU와 메모리 사용을 유발해서 비효율적이다.[^fn4]

임계구역 문제를 해결하기 위해 사용하는 동기화 도구가 있다:
- Semaphore
- Monitors
- Misc


## Semaphores (세마포)

세마포는 동기화 문제(임계구역 문제) 해결을 위해 만들어진 대표적인 동기화 도구/기술이다.[^fn6]

세마포는 두 가지 유형이 있다. 
- Binary Semaphore 
  - mutex lock이라고도 부른다. 
  - 0이나 1 두 값만 가질 수 있다. 
  - 1로 초기화한다. 
  - 여러 프로세스에서 생기는 임계구역 문제를 해결하기 위해 사용한다.
- Counting Semaphore 
  - 값의 범위가 따로 제한이 있지 않다. 
  - 여러 인스턴스가 자원에 접근하는 걸 통제할 때 사용한다. 



`acquire`, `release` 두 가지 동작이 존재한다. 
- 초기에는 P, V로 불리었다. 네덜란드에서 만들어져 네덜란드어의 약자다. 
- P는 test를 의미하고 `acquire()`로 사용된다. wait,sleep, down operation 이라고도 부른다. 
- V는 increment를 의미하고, `release()`로 사용한다. signal, wake-up, up operation 이라고도 부른다. 
- 두 연산 모두 atomic하다.

```java
class Semaphore {
  int value;      // number of permits
  Semaphore(int value) {
    // ...
  }
  void acquire() {
    value--;
    if (value < 0) {
      // add this process/thread to list
      // block
    }
  }
  void release() {
    value++;
    if (value <= 0) {
      // remove a process P from list
      // wakeup P
    }
  }
}
```

카운팅 세마포(counting semaphore)는 특정 자원/연산을 동시에 사용하거나 호출할 수 있는 스레드/프로세스의 수를 제한할 때 사용한다.[^fn5] 

세마포 클래스는 가상의 permit 개수(`value`)를 만들어서 내부 상태를 관리한다. 
외부 스레드/프로세스는 permit을 요청해 확보(acquire)하거나, 이전에 확보한 permit을 반납(release)할 수 있다. 

현재 사용할 수 있는 permit이 없는 경우 (value = 0), acquire메소드는 남는 permit이 생기거나, 인터럽트가 걸리거나, 지정한 시간을 넘겨 타임아웃이 걸리기 전까지 대기한다. release메소드는 확보했던 permit을 다시 세마포어에 반납한다. 

- `acquire()`는 value값을 감소시킨다. value값이 0보다 작으면, 이미 임계구역에 어느 프로세스가 존재한다는 의미다. 따라서 현재 프로세스는 접근하지 못하도록 막아야 한다. list(큐)라는 기다리는 줄에 추가한 뒤 block을 걸어준다. 

- `release()`는 value값을 증가시킨다. value값이 0보다 같거나 작으면 임계구역에 진입하려고 대기하는 프로세스가 list에 남아있다는 의미다. 이 프로세스 중 하나를 꺼내서 임계구역을 수행할 수 있도록 한다. 




프로세스 동기화를 위해 임계구역은 P(acquire)와 V(release) 사이에 위치한다. 
```
----- Process P -----
// Some code 
P(s); 
  // critical section 
V(s);
  // remainder section
```


세마포는 일반적으로 상호 배타(mutual exclusion)를 위해 사용한다. 


# References
[^fn1]:[codemcd님 velog - 프로세스 동기화](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-8.-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EB%8F%99%EA%B8%B0%ED%99%94-1){:target="_blank"}
[^fn2]:[geeksforgeeks - process synchronization](https://www.geeksforgeeks.org/introduction-of-process-synchronization/){:target="_blank"}
[^fn3]:[stackoverflow](https://stackoverflow.com/questions/33143779/what-is-progress-and-bounded-waiting-in-critical-section){:target="_blank"}
[^fn4]:[Wook's tistory](https://kpuls.tistory.com/51){:target="_blank"}
[^fn5]:[돼지왕왕돼지님 티스토리](https://aroundck.tistory.com/5873){:target="_blank"}
[^fn6]:[geeksforgeeks - semaphores](https://www.geeksforgeeks.org/semaphores-in-process-synchronization/){:target="_blank"}






