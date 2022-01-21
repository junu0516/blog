

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

<br>

## Reference

- Abraham Silberschatz, Greg Gagne, Peter B. Galvin *__Operating System Concepts__* , Wiley
