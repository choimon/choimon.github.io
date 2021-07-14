---
title: '스케줄러(Scheduler)'
last_modified_at: 2021-07-14T20:07
categories:
  - OS
tags:
  - scheduler
toc: true
toc_sticky: true
---
# Summary 
스케줄러에 대해 알아본다. 

# 스케줄러 (Scheduler)
어떤 프로세스에 자원을 할당할지를 결정하는 운영체제 커널의 모듈 및 시스템 소프트웨어 \
한정적인 메모리를 여러 프로세스가 효율적으로 사용할 수 있도록 실행할 프로세스를 선택한다

## 스케줄링 큐(Scheduling Queue)
프로세스는 수행하면서 상태가 여러 번 변하고 상태에 따라 서비스를 받아야 하는 곳이 다르다. 일반적으로 프로세스는 여러 개가 한 번에 수행 되므로 이에 따른 순서가 필요하다. 이러한 순서를 대기하는 곳을 큐라고 한다. 프로세스를 스케줄링 관리하는 큐에는 3가지 종류가 존재한다.[^fn1]


![scheulder image ]({{"/assets/images/posts/20210612_queuing_diagram.jpeg"| relative_url}})
[^fn2]
{: .text-right}

1. Job Queue
- 현재 시스템 안의 모든 프로세스의 집합[^fn3] 
- 하드디스크에 있는 프로그램이 실행되기 위해 메인 메모리의 할당 순서를 기다리는 큐[^fn6]
2. Ready Queue
- CPU 점유 순서를 기다리는 큐
- ready 상태의 메인메모리 안에 상주하며 CPU를 잡아서 실행되기를 기다리는 모든 프로세스의 
집합
- 새로운 프로세스는 이 큐에 담긴다.  
3. Device Queue
- Device I/O 작업을 대기하는 프로세스들의 집합 
- I/O를 하기 위한 여러 장치가 있는데, 각 장치를 기다리는 큐가 각각 존재한다.
- I/O장치가 현재 사용 불가해서(unavailable) blocked 된 프로세스들을 들고 있다.

![PCB]({{"/assets/images/posts/20210714_process_queue.png"| relative_url}})
[^fn6]
{: .text-right}

## 스케줄러 종류 
각각의 Queue에 프로세스들을 넣고 빼주는 스케줄러에는 3가지 종류가 존재한다. 

![scheulder image ]({{"/assets/images/posts/20210612_scheduler.png"| relative_url}})
[^fn4]
{: .text-right}

### 장기 스케줄러(Long-term scheduler/Job scheduler)
한정된 메모리에 많은 프로세스가 한꺼번에 메모리에 올라올 때, 대용량 메모리(디스크)에 임시로 저장된다. 이 큐에 저장된 프로세스 중 어떤 프로세스를 메모리에 할당하여 ready queue로 보낼지 결정한다.

- 메모리와 디스크 사이의 스케줄링 담당 
- 프로세스에 메모리 및 각종 리소스를 할당(admit)
- degree of multiprogramming 제어: 실행 중인 프로세스의 수 제어
  - degree of multiprogramming이 안정적이다면, 프로세스 생성 평균속도와 시스템을 나가는 프로세스 평균속도가 같다.[^fn2] 
- 프로세스 상태변화 
  - new -> ready(in memory)

- 모든 시스템이 지원하지 않는다. 
  - Time-sharing os는 장기 스케줄러가 없다. 바로 메모리에 올라가 ready 상태가 된다. 


### 단기 스케줄러(Short-term scheduler / CPU scheduler)
- CPU와 메모리 사이의 스케줄링을 담당
- 어떤 프로세스를 다음으로 실행할지(running) 결정한다. 
- ready 한 프로세스들 중 하나를 골라 실행시키고 CPU를 할당한다 (scheduler dispatch). 
- 장기 스케줄러보다 적은 degree of Multiprogramming 제어 제공
- 장기 스케줄러보다 빠르다 
- 프로세스 상태변화
  - ready -> running (-> waiting -> ready)

### 중기 스케줄러(Medium-term scheduler / Swapper)

- 여유 공간을 마련하기 위해 프로세스를 스와핑(swapping)한다. 
  - 실행하는(running) 프로세스는 I/O 요청을 받으면 정지될 수 있다. 정지된 프로세스(suspended process)는 더는 일을 완료할 수 없다. 이 정지된 프로세스를 메모리에서 지우고 다른 프로세스를 들일 공간을 만들기 위해서, 정지된 프로세스는 보조기억장치(secondary storage)로 옮겨진다. 이 과정을 **스와핑(swapping)**이라고 한다. 
  - 프로세스가 "swapped out" 또는 "rolled out" 되었다고 한다.  
- 다시 프로세스를 메모리에 데려와 실행을 계속한다.

- degree of Multiprogramming을 줄인다. 
- 속도는 단기와 장기 스케줄러의 중간이다. 
- 프로세스 상태변화
  - ready -> suspended
  - suspended process state: 외부적인 이유로 프로세스의 수행이 정지된 상태로 메모리에서 내려간 상태. 프로세스 전부 디스크로 swap out 된다. blocked 상태는 다른 I/O 작업을 기다리는 상태이기 때문에 스스로 ready state로 돌아갈 수 있지만, 이 상태는 외부적인 이유로 suspending 되었기 때문에 스스로 돌아갈 수 없다.
- Time sharing system에 들어있다. 

![process state image ]({{"/assets/images/posts/20210612_process_state.png"| relative_url}})
[^fn5]
{: .text-right}



# References
[^fn1]: [깃헙 인터뷰 문제](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS){:target="_blank"}
[^fn2]: [tutorialspoint](https://www.tutorialspoint.com/operating_system/os_process_scheduling.htm#:~:text=Long%20Term%20Scheduler,the%20memory%20for%20CPU%20scheduling.){:target="_blank"}
[^fn3]: [테리의 일상 블로그](https://dheldh77.tistory.com/entry/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%9F%ACScheduler){:target="_blank"}
[^fn4]: [geeksforgeeks](https://www.geeksforgeeks.org/difference-between-long-term-and-medium-term-scheduler/){:target="_blank"}
[^fn5]: [kosaf04phy님 블로그](https://kosaf04pyh.tistory.com/191){:target="_blank"}
[^fn6]: [codemcd님 velog](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-5.-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EA%B4%80%EB%A6%AC){:target="_blank"}