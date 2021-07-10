---
title: '운영체제 서비스'
last_modified_at: 2021-07-10T22:26
categories:
  - OS
tags:
toc: true
toc_sticky: true
---
# Summary 
운영체제는 사용자와 컴퓨터 하드웨어 사이에 통신 인터페이스(communication interface)로 일한다.[^fn1] 운영체제는 CPU, 메인 메모리, 하드디스크, 키보드, 마우스, 프린터 등 하드웨어 자원을 사용자 애플리케이션에 적절히 분배해주어 컴퓨터의 성능을 효율적으로 사용한다.  

운영체제의 하드웨어 자원을 관리하는 기능은 다음과 같이 나눌 수 있다. 
- 프로세스 관리
- 주기억장치 관리
- 파일 관리
- 보조기억장치 관리
- 입출력 장치 관리
- 네트워킹
- 보호
- 기타...

각 기능을 이 포스트에서 알아본다. 

# 프로세스 관리(Process management)

프로세스 관리는 운영체제 기능 중 가장 중요한 기능 중 하나다. 프로세스는 실제 메인 메모리에 할당되어 CPU를 사용하며 실행 중인 프로그램(program in execution)을 뜻한다.[^fn2]

운영체제는 프로세스 관리를 위해 다음과 같은 일을 한다. 
- 프로세스의 생성과 소멸(creation, deletion)
- 프로세스 활동 일시 중지, 활동 재개(suspend, resume)
- 프로세스 스케줄링 (process scheduling)
- 프로세스 간 통신(interprocess communication: IPC)
- 프로세스 간 동기화(synchronization)
- 교착상태 처리(deadlock handling)

프로세스 스케줄링에 관해서는 [이 포스트](../scheduler)를 참고하면 된다 
{: .notice}



# 주기억장치 관리(Main memory management)
주기억장치(메인 메모리)는 프로그램이 실행되기 위한 공간이다. 
CPU는 오직 메인 메모리에 있는 프로그램(실행되고 있을 때는 프로세스라 부른다)하고 소통할 수 있다. 

운영체제는 실행되는 프로세스를 메인 메모리와 디스크 사이로 왔다 갔다 옮기면서[^fn3], 메인 메모리를 효율적으로 사용하도록 관리한다.

- 프로세스에 메모리 공간 할당(allocation)
- 메모리의 어느 부분이 어느 프로세스에 할당되었는지 추적 및 감시 
- 프로세스 종료 시 메모리 회수(deallocation)
- 메모리의 효과적 사용
- 가상 메모리(virtual memory)를 사용해 실제 물리적 메모리보다 큰 용량을 사용 


# 파일 관리(File management)

<img src='{{"/assets/images/posts/20210710_disk_tracksector.png"| relative_url}}' alt='hard disk sector' style='width: 80%;' class='align-center'>
위 그림처럼 디스크는 물리적으로 track과 sector로 구성되어 있다[^fn4]. 반면에 파일은 논리적 데이터를 관리하는 것이다.
운영체제는 복잡한 과정으로 하드디스크에 저장된 것을 파일이라는 논리적 형태로 관리하여 사용자가 편리하게 사용할 수 있도록 보여준다. 
- 파일 생성과 삭제(file creation, deletion)
- 디렉토리 생성과 삭제(directory creation, deletion)
- 기본 동작(open, close, read, write, create, delete) 지원
- Track/sector - file 간의 매핑
- 백업

# 보조기억장치 관리 (Secondary storage management)
보조기억장치는 하드 디스크, 플래시 메모리 등이 있다. 
- 빈 공간 관리(Free space management)
- 저장공간 할당(Storage allocation)
- 디스크 스케줄링(Disk scheduling)


# 입출력 장치 관리(I/O device management)
입출력 장치는 키보드, 마우스, 프린터, 스피커, 마이크 등이 있다. 
- 장치 드라이브(Device drivers)
- 입출력 장치의 성능향상(buffering, caching, spooling)

# 시스템 콜(System call)

시스템 콜은 유저 프로세스에서 운영체제 서비스가 필요할 때 사용하는 프로그램 호출을 뜻한다. 시스템 콜은 프로세스와 운영체제 사이 인터페이스를 제공한다.[^fn5]
![System Call]({{"/assets/images/posts/20210710_systemcall.png"| relative_url}})
위 그림에서 Process1는 프로세스 관리에 시스템 콜을 요청한다. 프로세스가 실행되는 중간에 시스템 콜을 통해 필요한 운영체제 서비스의 코드로 점프할 수 있다. 

## 주요 시스템 콜 
- 프로세스 제어: end(정상 종료), abort(강제 종료), create, terminate, allocate and free memory [^fn6]
- 파일 관리: create, delete, open, close, read file
- 장치 관리: request, release, read, write, get/set attributes, attach/detach devices
- Information: get/set time, get/set system data
- Communication: socket, send, receive


# References
[^fn1]: [geeksforgeeks - OS functions](https://www.geeksforgeeks.org/functions-of-operating-system/){:target="_blank"}
[^fn2]: [codemcd님 velog](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-4.-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%84%9C%EB%B9%84%EC%8A%A4){:target="_blank"}
[^fn3]: [tutorialspoint](https://www.tutorialspoint.com/operating_system/os_memory_management.htm){:target="_blank"}
[^fn4]: [wikipedia - cylinder-head-sector](https://en.wikipedia.org/wiki/Cylinder-head-sector){:target="_blank"}
[^fn5]: [wikipedia - system call](https://en.wikipedia.org/wiki/System_call){:target="_blank"}
[^fn6]: [geeksforgeeks - systemcall](https://www.geeksforgeeks.org/introduction-of-system-call/){:target="_blank"}



