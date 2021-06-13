---
title: 'CPU 스케줄링(CPU Scheduling)'
last_modified_at: 2021-06-13T23:40
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


## CPU 스케줄링 종류 
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


### 비선점 스케줄링(Non-Preemptive Scheduling)
- Time-slice가 없다.[^fn3]
- CPU를 사용 중인 프로세스가 자율적으로 반납한다. 
- 프로세스가 자율적으로 CPU를 반납하는 시점에서 사용한다. 
- 스케줄링 알고리즘 
  - FCFS, SJF

### 선점 스케줄링(Preemptive Scheduling)
- 낮은 우선순위를 가진 프로세스보다 높은 우선순위를 가진 프로세스가 CPU를 선점한다. 
- 운영체제가 알고리즘에 따라 적당한 프로세스에 CPU를 할당하고, 필요할 때 회수한다. 
- 스케줄링 알고리즘 
  - SRT, RR, Priority Scheduling




# References
[^fn1]: [wikipedia](https://en.wikipedia.org/wiki/Scheduling_(computing)){:target="_blank"}
[^fn2]: [studytonight](https://www.studytonight.com/operating-system/cpu-scheduling#:~:text=CPU%20scheduling%20is%20a%20process,making%20full%20use%20of%20CPU.&text=The%20selection%20process%20is%20carried,scheduler%20(or%20CPU%20scheduler).){:target="_blank"}
[^fn3]: [hyunah030님 블로그](https://hyunah030.tistory.com/4){:target="_blank"}

