---
title: 'CPU 스케줄링(CPU Scheduling)'
last_modified_at: 2021-06-16T20:57
categories:
  - OS
tags:
  - cpu scheduler
toc: true
toc_sticky: true
---
# Summary 
CPU 스케줄링에 대해 알아봅니다.[^fn1]

# CPU 스케줄링

여러 프로세스가 CPU를 교환하여 생산적으로 동작할 수 있도록 스케줄링하는 과정이다. 시스템을 효율적으로, 빠르고, 공정하게 만드는 것을 목표로 한다. 


운영체제는 CPU가 일하지 않을 때마다 ready 큐에 있는 프로세스들 중 하나를 선택하여 실행해야 한다. 이 결정하는 과정(selection process)을 CPU 스케줄러가 책임진다. CPU 스케줄러는 어떤 프로세스를 CPU에 할당할지 결정한다. Dispatcher는 선택된 프로세스에 CPU 제어를 주는 모듈이다.[^fn2]




## CPU 스케줄러 
CPU와 메모리 사이의 스케줄링을 담당하는 단기 스케줄러(short-term scheduler)이다. 
메모리에 올라가 있는 ready 한 프로세스들 중 어떤 프로세스를 실행해서 CPU에 할당(allocate)할지 결정한다. 

## Dispatcher 
CPU 스케줄러가 선택한 프로세스에 CPU 제어를 주는 모듈이다. 
하는 일은 다음과 같다: 
- context switch 
- 유저 모드로 전환
- 유저 프로그램의 알맞은 장소로 이동(jump)해서 마지막으로 떠나기 전에 있던 장소로 프로그램을 재시작한다. 

디스패처는 매번 프로세스 전환(switch)이 일어날 때 사용됨으로 빨라야 한다. 한 프로세스를 멈추고 다른 프로세스를 시작하는 시간을 Dispatch Latency 라고 한다. 


## CPU 스케줄링 종류 및 알고리즘 

CPU 스케줄링은 다음과 같은 상황에 발생할 수 있다: 
1. 프로세스가 running 상태에서 waiting 상태로 전환할 때 
  - I/O 요청, wait요청을 통해 자식 프로세스를 종료할 때 
2. 프로세스가 running 상태에서 ready 상태로 전환할 때 
  - 인터럽트가 발생할 때 
3. 프로세스가 waiting 상태에서 ready 상태로 전환할 때 
  - I/O가 끝났을 때 
4. 프로세스가 종료할 때(terminate)
  - 부모 프로세스가 종료할 때 

1과 4같은 경우는 스케줄링 옵션이 없다. ready 큐에 존재하는 새로운 프로세스가 선택돼서 실행되어야 한다. 
반면에, 2와 3은 옵션이 있다. 


1과 4같은 경우에 일어나는 스케줄링을 비선점(non-preemptive) 스케줄링이라고 한다. 이 외의 케이스는 선점(preemptive) 스케줄링이라고 한다. 

### CPU 스케줄링 알고리즘의 목적 

**일반시스템**
- No Starvation
  - 각 프로세스들이 오랜시간동안 CPU를 할당받지 못하는 상황을 피한다. 
- Fairness
  - 각 프로세스에 공평하게 CPU를 할당한다. 
- Balance
  - 시스템 모든 부분을 바쁘게 활용한다. 

**일괄처리(Batch) 시스템**

요청시 즉각적으로 처리하는 것이 아니라, 일정기간 또는 일정량을 모았다가 한꺼번에 처리하는 방식 
- Throughput
  - 시간당 최대의 작업량을 낸다 
- Turnaround time 
  - 프로세스 생성부터 소멸까지의 시간을 최소화 한다. 
- CPU utilization 
  - CPU가 쉬는 시간이 없도록 한다. 

**대화형(Interactive) 시스템**

온라인과 같이 일에 대한 요청에 즉각적으로 처리하여 응답 받을 수 있는 시스템
- Response time 
  - 요청에 대한 응답시간을 줄인다. 


**Time Sharing 시스템**

각 프로세스에 CPU에 대한 일정시간을 할당하여 주어진 시간동안 프로그램을 수행할 수 있게하는 시스템 
- Meeting deadlines 
  - 데이터의 손실을 피하고, 끝내야하는 시간 안에 도달한다. 
- Predicatbility 
  - 멀티미디어 시스템에서의 품질이 저하되는 부분을 방지한다. 


### 비선점 스케줄링(Non-Preemptive Scheduling)
- Time-slice가 없다.[^fn3]
- CPU를 사용 중인 프로세스가 자율적으로 반납한다. 
- 프로세스가 자율적으로 CPU를 반납하는 시점에서 사용한다. 
- 스케줄링 알고리즘 
  - FCFS, SJF

#### FCFS(First Come First Served)
- 먼저 CPU를 요처하는 프로세스를 먼저 처리한다.[^fn4]
- 예) P1, P2, P3 순으로 프로세스가 CPU를 요청하면, P1,P2,P3 순으로 실행된다. 

#### SJF(Shortest Job First)
- 버스트 시간(burst time)이 짧은 프로세스부터 CPU를 할당한다. 
- 평균 waiting time을 최소하 하기 위해 사용된다. 
- 버스트 시간이 긴 프로세스는 오랜 시간 기다려야되서, No Starvation 룰을 위반한다. 

### 선점 스케줄링(Preemptive Scheduling)
- 낮은 우선순위를 가진 프로세스보다 높은 우선순위를 가진 프로세스가 CPU를 선점한다. 
- 운영체제가 알고리즘에 따라 적당한 프로세스에 CPU를 할당하고, 필요할 때 회수한다. 
- 스케줄링 알고리즘 
  - SRT, RR, Priority Scheduling




# References
[^fn1]: [wikipedia](https://en.wikipedia.org/wiki/Scheduling_(computing)){:target="_blank"}
[^fn2]: [studytonight](https://www.studytonight.com/operating-system/cpu-scheduling#:~:text=CPU%20scheduling%20is%20a%20process,making%20full%20use%20of%20CPU.&text=The%20selection%20process%20is%20carried,scheduler%20(or%20CPU%20scheduler).){:target="_blank"}
[^fn3]: [hyunah030님 블로그](https://hyunah030.tistory.com/4){:target="_blank"}
[^fn4]: [혀나_0_0님 블로그](https://l.tistory.com/4)