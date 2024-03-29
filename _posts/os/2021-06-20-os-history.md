---
title: '운영체제 역사'
last_modified_at: 2021-06-20T23:19
categories:
  - OS
tags:
  - interrupt
toc: true
toc_sticky: true
---
# Summary 
운영체제 역사에 대해 알아본다. 

# 1세대(The First Generation): 1940-1950년대 초기
컴퓨터가 1940년대에 처음 나왔을 때는 운영체제가 없었다.[^fn1]
이 시기에는 컴퓨터가 단순한 수학 계산을 위해 사용되었기 때문에 운영체제가 필요하지 않았다. 대신에 오퍼레이터(operator)라는 컴퓨터를 사용하는 직업이 따로 있었다. 컴퓨터의 크기도 커서 건물전체를 사용해야할 정도였다. 운영체제는 컴퓨터가 발전하면서 같이 발전하게 되었다. 

## 초기 컴퓨터 
초기 컴퓨터는 카드리더, 프로세서, 프린터 이렇게 크게 3가지로 구성되었다.[^fn2]
- 카드 리더: 입력기이다. 입력은 종이에 입력할 코드에 맞는 구멍을 뚫어서 넣어주었다. 
- 프로세서: 현재와 비슷하게 계산을 담당했다.
- 프린터: 입력에 대한 결과를 종이에 찍어서 보여주었다. 

오퍼레이터가 프로그램을 수행할 때마다 직접 컴파일,링크,로딩 순서를 입력해줘야했다.

# 2세대(The Second Generation): 1955-1965
첫 운영체제는 1950년대 초기에 나왔다. GMOS라고 불렸으며, General Motors 회사에서 IBM 701 머신을 위해 만들었다. 
1950년대 운영체제는 단일 스트림 일괄 처리 시스템(single-stream batch processing system)이라고 불리었다. 비슷한 잡(job)들이 그룹(groups/batches)핑 되어 운영체제로 보내졌다. 운영체제는 펀치카드(punch card)

이후 만들어진 기계들은 메인프레임(mainframe)이라고 불렸고, 큰 컴퓨터 방에서 전문 오퍼레이터들이 사용했다. 이때 기계들이 무척 비쌌기 때문에, 큰회사나 정부 단체에서만 사용할 수 있었다. 

## Batch processing system(일괄 처리 시스템)
과거에는 오퍼레이터가 직접 프로그램을 수행할 때마다 컴파일,링크,로딩 순서를 입력해야 했었는데, 이 작업을 자동화 한것이 일괄 처리 시스템이다. 
이 과정을 하나의 프로그램으로 작성하여 프로세서의 메모리안에 할당해주었다. 이 프로그램이 항상 프로세서안에서 거주(reside)한다는 의미로 resident monitor라고 불리었다. 

# 3세대(The Third Generation): 1965-1980
1960년대 후반에 운영체제 디자이너들은 다중 프로그래밍 시스템을 개발할 수 있었다. 다중 프로그래밍 시스템에서는 컴퓨터 프로그램이 여러 일(job)을 동시에 수행할 수 있다. 다중 프로그래밍은 운영체제 개발에서 큰 역할을 했다. 다중 프로그래밍으로 CPU가 거의 100퍼센트 바쁠 수 있게 구동가능했기 때문이다. 

또한 1961년 DEC PDP-1로 시작으로 이 시기에는 미니컴퓨터(minicomputer) 개발이 성장했다. PDP-1은 18-bit words 에 4K 였지만 $120,000달러나 했다. 이런 마이크로컴퓨터(microcomputer)는 불티나게 팔렸고, 새로운 산업을 만들었다. 이런 PDP기계들이 개인 컴퓨터(personal computer)가 4세대에 탄생할 수 있게 만들었다.

## Multiprogramming system(다중 프로그래밍 시스템)
### 탄생배경
과거 컴퓨터의 메모리에는 residnet monitor를 제외하고 단 하나의 애플리케이션만을 할당하여 사용했다. 
프로그램을 수행하는 도중에 CPU와 I/O장치가 교대로 동작하는데, I/O장치가 수행하는 동안에는 CPU가 아무것도 할 일이 없었다. 이렇게 CPU가 아무 일도 안하는 상태를 idle 상태라고 일컫는다. I/O장치는 CPU에 비해 매우 느리기때문에 idel상태의 비율이 너무 높았다. 이런 비효율적 수행을 해결하기 위해 다중 프로그래밍 시스템이 나왔다.

### 동작
다중 프로그래밍 시스템은 메모리에 여러 애플리케이션(프로그램)을 올리는 시스템이다
- 예) cat1, cat2 라는 2개의 애플리케이션이 있다고 가정한다. 처음에 cat1에서 CPU수행을 하다가 I/O장치 수행으로 넘어간다. 이 순간 CPU는 idle 상태가 아니라 cat2의 CPU 수행을 시작한다. idle 상태의 시간을 최대한 줄이는 것이다. 

### 문제
메모리에 여러 프로그램을 올리면서 문제점이 발생했다. 
- CPU가 어느 프로그램을 수행하는지 선택해야 하는 문제 (CPU 스케줄링)
- 새로 들어온 프로그램을 어느 메모리 공간에 할당해야하는 문제 

위와 같은 문제들은 운영체제의 중요한 역할들이다. 

## Time-sharing system(시분할 시스템)


### 탄생배경
모니터와 키보드를 사용하게 되면서 사용자와 컴퓨터 사이에 대화 형식이 가능해졌다. 
단말기(terminal)형태로 사용했다. 

각 사용자들은 모니터와 키보드만을 가지지만, 실제 프로세서는 하나의 단말기에 존재해서 이를 공유하여 사용했다. 이와 같은 단말기 형태에서 다중 프로그래밍은 문제가 생긴다. 
예)사용자 1,2,3이 있고 단말기 메모리에 user1 프로그램, user2 프로그램, user3 프로그램이 할당 되어있다. CPU는 하나다. 다중프로그래밍에서는 user1이 CPU를 수행하고 있는 도중에 다른 사용자는 CPU를 사용할 수 없다. 다른 사용자들은 user1이 CPU수행을 모두 마치거나 I/O를 마칠 때까지 기다려야 한다. 매우 비효율적이다. 이를 해결하기 위해 나온 것이 시분할 시스템이다 

첫 시분할 시스템 개념은 1954년에 John Backus가 MIT 여름 세션에서 언급했다고 전해진다. 1959년에 MIT의 John McCarthy의 첫 시분할 시스템 프로젝트 시작으로, 1960-1970년대에는 메인프레임 컴퓨터에 개발되었다. 1980년대 초반 마이크로컴퓨팅의 인기 상승으로 시분할 시스템은 상대적으로 주목받지 못했지만, 인터넷으로 다시 시분할 개념이 주목받았다. 하지만 이후 저렴해진 가격으로 개인컴퓨터(personal computer)가 들어서면서 시분할 시스템은 인기가 줄어들기 시작했다.[^fn3]
{: .notice}

### 동작 
시분할 시스템은 CPU가 하나의 프로그램을 수행하는 시간을 제한하여 여러 사용자나 여러 작업이 시간 단위로 공유/분할하여 사용한다.
- 예) user1 프로그램이 일정 시간 수행하면 끝내지 못했더라도 반드시 다음 프로그램으로 넘어가서(switching) 또 다시 일정 시간을 수행한다. 모든 프로그램이 일정 시간을 공평하게 수행하게 한다. 
이 일정 시간은 매우 짧은 시간(ms)이여서 CPU가 하나인 환경에서도 여러 사용자가 동시에 사용하는 듯한 효과를 가질 수 있다. 

시분할 시스템을 사용하면서 여러 사용자(프로세스)간에 통신이 가능해졌다. 

### 문제 
다중프로그래밍처럼 스케줄링과 메모리 문제가 있었다. 
이를 해결하기 위해 여러 기술들(동기화 기술, 가상 메모리 등)이 이후에 나왔다. 



# 4세대(The Fourth Generation): 1980-현재
4세대에서는 개인 컴퓨터(personal computer)가 탄생했다. 개인 컴퓨터는 3세대 미니컴퓨터와 비슷했지만 가격은 훨씬 쌌다. 

개인 컴퓨터 탄생에는 마이크로소프트(Microsoft)사와 윈도우즈 운영체제(Windows OS) 출현이 큰 역할을 했다. 윈도우즈는 1975년도에 폴 앨런과 빌게이츠가 개인컴퓨터의 미래를 크게보고 만들어졌다. 1981년도에 폴 앨런과 빌 게이츠는 MS-DOS를 출시했다. 그 이후로, Windows 95, Windows 98, Windows XP 등 여러 운영체제를 출시했다. 

애플도 1980년대에 운영체제를 출시했다. 스티브 잡스는 애플 맥킨토시를 만들었고, 이 상품은 사용자 친화적이여서 큰 성공을 거뒀다. 

## 인터럽트 기반 시스템(Interrupt based system)
현대 운영체제는 인터럽트 기반 시스템이다. 

### 인터럽트 
CPU에게 보내는 전기 신호다. 
컴퓨터 전원이 들어오고, 부팅 과정이 끝나고 운영체제가 동작하는 순간 수 많은 인터럽트가 발생할 수 있다. 인터럽트가 발생하면 CPU는 하던 일을 중지하고 해당 인터럽트를 처리하기 위해 운영체제 내부에 있는 ISR로 이동하여 수행한다. 그리고 수행이 끝나면 원래 위치로 돌아간다. 인터럽트는 하드웨어 인터럽트, 소프트웨어 인터럽트, 내부 인터럽트로 총 세 가지가 존재한다.


**하드웨어 인터럽트(Hardware Interrupt)**

인터럽트라고 하면 기본적으로 하드웨어 인터럽트를 뜻한다. 
예) 사용자가 마우스를 움직이는 것, 키보드 입력
- 사용자가 움직이는 순간 마우스에서 인터럽트 전기 신호가 발생하여 이를 CPU에 보낸다. CPU는 이를 감지하고, 자신이 하던 일을 멈춘 후에 이 인터럽트 신호를 처리하기 위해 운영체제 내부에 있는 인터럽트를 처리하는 코드(Interrupt Service Routine, ISR)로 이동한다. 이 코드 안에 마우스가 움직일 때 동작해야하는 내용이 담겨있고, 이를 수행한다. 


**소프트웨어 인터럽트(Software Interrupt)** 

소프트웨어에서 요청하는 인터럽트 이다. 
소프트웨어 인터럽트는  명령어로 직접 인터럽트 전기 신호를 CPU에 보낸다. 
프로그램에서 `swi`, `int`와 같은 어셈블리어 명령어를 수행한다. 이 명령어는 운영체제마다 다르다. 
예) 마우스로 워드 작성 프로그램을 실행하는 것 
- 마우스가 더블클릭으로 실행시킨다면, 워드 프로그램을 실행시키는 것까지는 하드웨어 인터럽트가 수행된다
- 워드 프로그램에서 사용자가 하드디스크에 있는 다른 워드 파일을 가져오고 싶은 경우, 소프트웨어 인터럽트가 발생한다. 하드웨어 인터럽트와 동일하게 소프트웨어 인터럽트를 처리하는 코드(ISR)가 운영체제 내부에 존재한다. 이 코드 안에서 하드디스크에서 읽을 파일을 찾아서 반환해주는 일을 처리한다. 

**내부 인터럽트(Internal Interrupt)**

프로그램을 수행하는 도중에 발생하는 예외 상황을 처리한다.
- 예) 0으로 나누는 동작 \
  프로그램의 내부에 result = a / 0; 와 같은 코드가 있을 때, CPU는 내부 인터럽트를 발생시켜 운영체제 안에 있는 DividedByZero라는  ISR로 이동한다. 이 곳에서 잘못된 동작을 수행한 프로그램을 강제로 종료시킨다.


### 동작 
운영체제는 평소에는 대기 상태에 있다가 인터럽트가 발생하는 순간 작업을 수행한다. 그 인터럽트의 종류에 따라 운영체제 내부에 위치한 ISR로 이동하여 그에 맞는 처리를 한다. 
![Interrupt based system]({{"/assets/images/posts/20210620_interrupt.png"| relative_url}})


애플리케이션이 실행하는 중에도 계속해서 인터럽트가 발생한다. 워드 프로그램에서 키보드로 글을 작성하거나, 하드디스크에 있는 파일을 불러올 때 모두 인터럽트가 발생한다. CPU는 애플리케이션과 운영체제 내부를 교대로 수행한다.

# References
[^fn1]: [optsytms](https://sites.google.com/site/optsytms/history-of-operating-systems){:target="_blank"}

[^fn2]: [codemcd님 velog](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-2.-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%97%AD%EC%82%AC){:target="_blank"}
[^fn3]: [wikipedia](https://en.wikipedia.org/wiki/Time-sharing#Development){:target="_blank"}

[^fn4]: [javatpoint](https://www.javatpoint.com/history-of-operating-system){:target="_blank"}




