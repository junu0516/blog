

+++
title = "Operating System Concepts - Chapter 3 요약(작성중)"
tags = ["운영체제"]
categories = ["Computer Science"]
date = "2022-01-18"

+++

공룡책 Ch 3 요약 (원서를 읽고 요약하는 과정에서 잘못된 내용이 있을 수 있습니다.)

![img](https://media.wiley.com/product_data/coverImage300/66/11198003/1119800366.jpg)

<br>

## 1. Process Concept

### 1-1. The Process

- 프로세스는 메모리에 올라가 실행중인 프로그램을 의미함(__program in execution__)
- 프로세스의 현재 실행에 관한 정보는 프로그램 카운터와 프로세서의 레지스터 구성으로 나타낼 수 있음
- 프로세스의 메모리 구조는 크게 텍스트, 데이터, 힙&스택 영역으로 나눌 수 있음
  - 텍스트 영역 : 실행 가능한 코드 데이터가 저장됨
  - 데이터 영역: 정적/전역 변수가 저장됨
  - 힙 영역: 프로그램 런타임 시점에 동적으로 생성된 객체 등이 저장됨
  - 스택 영역: 함수 호출시 일시적으로 매개변수, 리턴 주소, 지역변수 등이 저장됨

<img src="https://user-images.githubusercontent.com/68586291/150096476-71988588-c961-4e7a-9f6c-b73179ec0379.png" alt="image" style="zoom:70%"/>

- 텍스트 및 데이터 영역의 크기는 고정되어 있으며, 스택과 힙 영역은 프로그램 실행 시점에 크기가 가변적임

  - 함수가 호출될 때마다 지역변수, 매개변수, 리턴주소 등을 포함한 __`activation record`__ 가 스택에 추가(push)됨
  - 비슷하게 객체가 생성되면 힙 메모리 영역에 적재되며, 쓰임이 다한 후 메모리의 해제외 함께 힙 영역의 크기도 줄어듦

- 프로세스 자체가 다른 코드의 실행 환경이 될 수도 있음

  - __`JVM`__ 이 대표적인 예로, 가상머신이 로드된 자바 코드를 읽어들인 후 가상머신 자체의 명령어를 통해 코드 대신 행동을 취하는 방식임

    *The JVM executes as a process that interprets the loaded Java code and takes actions(via native machine instructions) on be half that code.*

  - 예를 들어 __`java Program`__ 이라는 명령을 실행했을 때, __`java`__ 는 JVM을 프로세스로서 실행시킨다는 것을 의미하며, __`Program`__ 은 JVM 환경에서 해당 프로그램을 실행시킨다는 것을 의미함

​    

### 1-2. Process State

<img src="https://user-images.githubusercontent.com/68586291/151744076-5dfe2474-a5c1-4368-a457-120ba7a7ca25.png" style="zoom:80%"/>

- 프로세스가 실행된다는 것은 프로세스의 상태(state)가 바뀐다는 것을 의미
- 프로세스의 상태는 __`생성(New)`__ , __`실행(Running)`__ , __`대기(Waiting)`__ , __`준비(Ready)`__ , __`종료(Terminated)`__ 의 5가지로 나뉨
  - 생성: 프로세스가 말 그대로 새롭게 생성된 상태를 의미
  - 실행 : 명령(Instructions)들이 실행되는 상태를 의미
  - 대기 : 프로세스가 입출력과 같은 특정 이벤트를 위해 대기하는 상태를 의미
  - 준비 : 프로세스가 실행을 위해 프로세서에 할당되는 것을 기다리는 상태를 의미
  - 종료 : 프로세스가 모든 실행을 종료한 상태를 의미
- 프로세스 혹은 코어 하나당 하나의 프로세스가 실행 상태가 될 수 있음


<br>

### 1-3. Process Control Block

<img src="https://user-images.githubusercontent.com/68586291/151744163-c3ffb8e7-2bf0-41d3-a198-e9f8001d61e3.png"  style="zoom:80%"/>

- 각각의 프로세스는 운영체제 상에서 프로세스 제어 블록(__`PCB`__) 으로 대표되며, 여기에는 개별 프로세스의 구체적인 정보들이 포함되어 있음
- 프로세스 상태(__`Process State`__) : 바로 위에서 언급한 생성,준비,실행,대기,종료 등의 상태 정보
- 프로그램 카운터(__`Program Counter`__) : 카운터는 프로세스 내에서 다음에 수행되야 할 명령의 주소를 가리킴
- 일반 레지스터 및 CPU 레지스터(__`Registers`__) : __`레지스터`__ 는 프로세서에서 자료를 보관하기 위한 장소로 사용되며, 위의 구조에서는 일반적인 레지스터와 함께 스택 포인터나 상태 코드 정보 등을 저장하는 레지스터가 포함되있음
  - 인터럽트 발생시 레지스터에 상태 정보가 제대로 저장되어 있어야, 나중에 프로세스가 재개될 때 정확한 상태로 수행될 수 있음

- CPU 스케줄링 정보(__`CPU-scheduling information`__) : 스케줄링 관련 정보를 포함하며, 여기에는 프로세스 우선순위나 스케줄링 큐에서 가리킬 포인터 등이 있음
- 메모리 관리 정보(__`Memory-management information`__) : 페이지 및 세그먼트 테이블이나 레지스터의 공간 제한 등의 프로세스 메모리 관련 정보를 포함하고 있음
- 프로세스 계정 정보(__`Accounting information`__) : CPU 사용량, 시간 제한, 계정 혹은 프로세스 번호 등의 정보를 포함하고 있음
- 입출력 상태 정보(__`I/O status information`__) : 프로세스에 할당된 입출력 디바이스 목록이나, 열린 파일 목록 등의 정보를 포함하고 있음

<br>

### 1-4. Threads

- 스레드는 프로세스 내에서 개별 실행 흐름의 작은 단위를 의미함

- 지금 까지 언급된 프로세스는 엄밀히 말해 싱글 스레드 기반의 수행(__`single thread of execution`__) 을 말하는 것이며, 오늘날 보통의 프로세스 컨셉은 프로세스가 다수의 스레드를 사용하는 것을 허용함
- 이는 특히 멀티코어 환경에서 여러 스레드가 병렬적으로 작업을 수행하게 할 수 있다는 점에서 이점이 있음
- 멀티 스레딩을 지원하는 환경에서 프로세스 제어 블록은 개별 스레드의 정보까지 포함하는 식으로 확장됨 

<br>

## 2. Process Scheduling

- 멀티 프로그래밍의 목적은 결국 CPU 자원의 활용을 최대한으로 높이고자 프로세스가 항상 실행될 수 있도록 하는 데 있음
- 이를 위해 프로세스 스케줄러(__`process scheduler`__) 은 적절한 프로세스들을 선택하여 자원을 분배하여 실행될 수 있도록 함 

<br>

## Reference

- Abraham Silberschatz, Greg Gagne, Peter B. Galvin *__Operating System Concepts__* , Wiley
- [위키피디아 프로세서 레지스터](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EC%84%B8%EC%84%9C_%EB%A0%88%EC%A7%80%EC%8A%A4%ED%84%B0)
- [위키피디아 프로세스 제어 블록](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4_%EC%A0%9C%EC%96%B4_%EB%B8%94%EB%A1%9D)
