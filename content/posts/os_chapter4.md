

+++
title = "Operating System Concepts - Chapter 4 요약"
tags = ["운영체제"]
categories = ["Computer Science"]
date = "2022-02-03"

+++

공룡책 Ch 4 요약 (원서를 읽고 요약하는 과정에서 잘못된 내용이 있을 수 있습니다.)

![img](https://media.wiley.com/product_data/coverImage300/66/11198003/1119800366.jpg)

<br>

## 1. Thread Overview

<img src="https://user-images.githubusercontent.com/68586291/152319022-c93157d6-0d13-4b96-9940-77003b2c039f.png" alt="image" style="zoom:70%"/>

- 한 번에 하나의 작업만을 수행하는 전통적 의미의 단일 프로세스는 싱글 스레드 방식이 되며, __프로세스가 여러 개의 스레드를 가지게 될 경우 동시에 여러 작업을 수행할 수 있음__

<img src="https://user-images.githubusercontent.com/68586291/152320101-14875fda-5442-4441-abbe-3bea6ad7f8a8.png" alt="image" style="zoom:70%"/>

- 웹서버(__`Web Server`__)는 대표적인 멀티스레딩 구조로, 클라이언트의 여러 요청이 동시다발적으로 들어올 때 여러 스레드가 이를 동시에 처리함으로써 한 번에 하나씩 처리할 때보다 더욱 빠르게 응답할 수 있음
- 스레드는 스레드 아이디, 프로그램 카운터(__`PC`__), 레지스터 집합(__`Register Set`__) , 스택 메모리 공간(__`Stack`__) 을 가지고 있는 프로세스 내부의 작업 실행 단위를 말함
- 언급한 요소들을 프로세스로부터 프로세스가 개별적으로 할당받아 사용하게 되며, 이외의 코드(__`Code`__) , 전역 데이터 및 파일(__`Data`__ , __`Files`__ )은 여러 스레드가 공유하는 구조임

​    

### 1-1. Benefits

> 멀티스레딩 방식의 장점에 대해 알아보자

- __Responsiveness__ : 한 번에 여러 개의 작업을 처리할 수 있기 때문에 하나의 작업에 문제가 있더라도 다른 작업은 계속해서 수행할 수 있다는 점에서 높은 반응성을 보임

  (멀티스레딩 방식의 웹서버를 생각하면 됨)

- __Resource Sharing__ : 여러 스레드는 하나의 프로세스가 가진 메모리와 자원을 공유하며,  이를 통해 여러 스레드의 작업이 동일한 공간(__`same address space`__)에서 일어나도록 할 수 있음

- __Economy__ : 여러 스레드가 프로세스의 자원을 공유하는 방식은, 다른 작업을 수행하기 위해 별도로 프로세스를 만들고 이를 위한 __`Context-Switching`__ 을 수행하는 것 보다 훨씬 경제적임

- __Scalability__ : 멀티 프로세서 구조에서 멀티 스레딩을 적용할 경우, 여러 프로세서(코어)에 각각의 스레드가 병렬적으로(paralell) 돌아가게 함으로써 더욱 확장성 높은 구조로 발전시킬 수 있음

  (*The benefits of multithreading can be even greater in a multiprocessor architecture, where threads may be running in parallel on different processing cores.*)

<br>

## 2. Multicore Programming

<img src="https://user-images.githubusercontent.com/68586291/152324481-74fe6a45-d0f3-47f3-896b-993399ffcbf4.png" alt="image" style="zoom:80%"/>

- 멀티코어(__`multicore`__) 시스템은 하나의 칩(__`processing chip`__)에 여러 개의 코어(__`computing cores`__) 가 붙어 있는 구조로, 여기서 __운영체제는 각각의 코어를 개별적인 CPU 자원으로 인식하게 됨__

- 개별 코어는 __한 번에 하나의 스레드 작업만을 처리할 수 있으며__ , 멀티 코어 환경에서의 동시성(__`concurrency`__) 의 의미는 결국 여러 코어에 각각의 스레드 작업을 할당하여 동시에 실행될 수 있도록 하는 것을 의미함

- 여기서 병렬성(__`parallelism`__) 과 동시성(__`concurrency`__) 을 구분하여 알 필요가 있는데, 병렬성은 동시에 하나 이상의 작업을 수행할 수 있는 것을 의미하며 동시성은 여러 작업이 모두 처리될 수 있도록 여러 작업을 모두 지원하는 것을 의미함

- 동시성의 경우 '동시'라는 것이 정말로 정확히 같은 시간에 여러 작업을 처리하는 것을 의미하는 것이 아닌, 문맥 교환(__`Context Switching`__)을 통해 여러 작업을 잘게 쪼개서 순차적으로 처리함으로써 결국 모든 작업이 고르게 실행되는 식으로 흘러가도록 하는 것을 의미함.

  - 따라서, 병렬성이 굳이 담보되지 않아도 동시성을 충족시키는 것이 가능함.
  - 한 번의 하나의 작업만 처리할 수 있는 상황에서 여러 프로세스 간의 잦은 문맥교환을 통해 동시성을 만족시키면, 결과적으로 모든 프로세스가 동시에 동일하게 동작하는 것 같이 보이지만 실제로 병렬 처리된 것은 아닌 것

  (*A concurrent system supports more than one task by allowing all the tasks to make progress. In contrast, a paralel system can perform more than one task simultaneously.*)

​    

### 2-1. Programming Challenges

​    

<br>

## Reference

- Abraham Silberschatz, Greg Gagne, Peter B. Galvin *__Operating System Concepts__* , Wiley
- [위키 피디아 병렬 컴퓨팅](https://ko.wikipedia.org/wiki/%EB%B3%91%EB%A0%AC_%EC%BB%B4%ED%93%A8%ED%8C%85)
- [멀티 코어 프로세서란? - 동커벨](https://donghoson.tistory.com/21)
- [동시성과 병렬 처리의 차이점](https://ko.gadget-info.com/difference-between-concurrency)
