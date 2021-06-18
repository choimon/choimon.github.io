---
title: '동기와 비동기, Blocking vs Non-blocking'
last_modified_at: 2021-06-18T22:23
categories:
  - OS
tags:
  - sync/async
  - blocking/nonblocking I/O
toc: true
toc_sticky: true
---
# Summary 
동기와 비동기의 차이점을 알아보고, 
Blocking과 Non-blocking I/O 모델에 대해서도 알아본다. 

# Synchronous vs Asynchronous (동기와 비동기의 차이)
결과물을 돌려받는 시점이 다르다. \
동기는 메소드를 실행시킴과 동시에 반환 값이 기대된다. [^fn1] 반면에, 비동기는 동시에 바로 반환 값이 기대되지 않다. 
- 동시: 실행되었을 때 값이 반환되기 전까지는 blocking 되어 있다.
- 비동기: blocking 되지 않고(non-blocking) 이벤트 큐에 넣거나, 백그라운드 스레드에 해당 task를 위임하고 바로 다음 코드를 실행한다.

## Synchronous 동기 
- System call이 끝날때까지 기다리고 결과물을 가져온다.
- 동기 방식은 설계가 간단하고 직관적이다. 하지만, 결과가 올때까지 아무것도 못하고 대기해야 된다. 

## Asynchronous 비동기 
- System call이 완료되지 않아도 나중에 완료가 되면 그때 결과물을 가져온다.
  - 주로 Callback 함수를 통해 결과물을 가져온다. 
- 비동기 방식은 동기보다 복잡하지만, 결과가 주어지는데 시간이 걸리더라도 그 시간 동안 다른 작업을 할 수 있으므로, 자원을 효율적으로 사용할 수 있다


# Blocking vs Non-blocking I/O Model 
blocking과 non-blocking의 차이는 프로그램이 바로 실행할 수 있는가/없는가 이다.(프로그램의 실행하는 순서 관점)


## Blocking I/O Model
blocking: 어떤 이벤트가 끝날 때까지 기다린다. 
- I/O 작업이 진행되는 동안 유저 프로세스는 자신의 작업을 중단한 채 대기한다. 
  - System call 끝날때까지 프로그램은 대기해야 한다. System call 완료가 되면 리턴한다. 
- I/O 작업은 User level(application)에서 직접 수행 할 수 없고, Kernel level(OS)에서 일어나는 과정이다. [^fn2]
  - 유저 프로세스(application)은 커널에 I/O작업에 대한 요청을 해야한다. 
  - I/O 작업 처리를 위해 user level application이 시스템 함수를 호출한다(system call). 이 때 context-switching이 발생한다. 
  - Kernel level에서 해당 I/O작업이 끝나고 데이터를 반환하면 그 때 애플리케이션 단의 스레드에 걸렸던 block이 풀린다. 
  - 애플리케이션 관점에서는 아무런 동작 안하고 멈춘 것 처럼 보이지만, 실제로는 커널에서 I/O작업을 수행하느라 block이 되어있는 상태. 

- Wait Queue에 들어간다.

- cpu의 기본적인 I/O 모델로 리눅스에서 모든 소켓 통신은 기본 blocking으로 동작한다.

- 예) C언어의 scanf()를 사용하는 프로그램에서 사용자가 입력하기 전까지 대기한 상태로 scanf는 리턴하지 않는다. 

### Synchronous vs Blocking 
- Wait Queue에 존재하는 지 아닌지 차이[^fn3]
- Synchronous은 System Call의 Return을 기다리는 동안 Wait queue에 머물 수도 아닐 수도 있다.
  - 작업을 요청한 후 작업의 결과가 나올 때까지 기다린 후 처리한다. 
  - Synchronous는 blocking 이거나 non-blocking 일 수 있다.[^fn4] 
- Blocking은 System Call의 Return을 기다리는 동안 필수로 Wait queue에 머문다.
  - I/O가 끝날 때가지 대기해야 한다. 
  - 끝나기 전에는 함수가 반환되지 않는다. 
  - blocking은 언제나 synchronous 하다. 




## Non-blocking I/O Model
- System call이 완료되지 않아도 대기하지 않고 리턴해버린다. 
- wait queue에 들어가지 않는다. 
- Async처럼 callback이 없다. 


### Asynchronous vs Non-blocking 
- System call이 즉시 return 될 때 데이터의 포함 유무 
- Asyncrhonous는 요청에 처리 완료와 관계없이 응답한다. 이후 운영체제에서 응답할 준비가 되면 응답한다. 
- Non-blocking은 요청에 처리할 수 있으면 바로 응답하고 아니면 Error를 반환한다. 





# References

[^fn1]: [깃헙 인터뷰](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#%EB%8F%99%EA%B8%B0%EC%99%80-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%9D%98-%EC%B0%A8%EC%9D%B4){:target="_blank"}
[^fn2]: [_Jbee님 블로그](https://asfirstalways.tistory.com/348){:target="_blank"}
[^fn3]: [Nesoy님 블로그](https://nesoy.github.io/articles/2017-01/Synchronized){:target="_blank"}
[^fn4]:[stackoverflow](https://stackoverflow.com/questions/8416874/whats-the-differences-between-blocking-with-synchronous-nonblocking-and-asynch#:~:text=Summary%3A%20Blocking%20is%20always%20synchronous,before%20proceeding%20for%20next%20instruction.){:target="_blank"}


