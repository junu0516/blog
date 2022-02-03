

+++
title = "Operating System Concepts - Chapter 4 요약"
tags = ["운영체제"]
categories = ["Computer Science"]
date = "2022-02-02"

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

<img src="https://user-images.githubusercontent.com/68586291/152324481-74fe6a45-d0f3-47f3-896b-993399ffcbf4.png" alt="image" style="zoom:85%"/>

- 멀티코어(__`multicore`__) 시스템은 하나의 칩(__`processing chip`__)에 여러 개의 코어(__`computing cores`__) 가 붙어 있는 구조로, 여기서 __운영체제는 각각의 코어를 개별적인 CPU 자원으로 인식하게 됨__

- 개별 코어는 __한 번에 하나의 스레드 작업만을 처리할 수 있으며__ , 멀티 코어 환경에서의 동시성(__`concurrency`__) 의 의미는 결국 여러 코어에 각각의 스레드 작업을 할당하여 동시에 실행될 수 있도록 하는 것을 의미함

- 여기서 병렬성(__`parallelism`__) 과 동시성(__`concurrency`__) 을 구분하여 알 필요가 있는데, 병렬성은 동시에 하나 이상의 작업을 수행할 수 있는 것을 의미하며 동시성은 여러 작업이 모두 처리될 수 있도록 여러 작업을 모두 지원하는 것을 의미함

- 동시성의 경우 '동시'라는 것이 정말로 정확히 같은 시간에 여러 작업을 처리하는 것을 의미하는 것이 아닌, 문맥 교환(__`Context Switching`__)을 통해 여러 작업을 잘게 쪼개서 순차적으로 처리함으로써 결국 모든 작업이 고르게 실행되는 식으로 흘러가도록 하는 것을 의미함.

  - 따라서, 병렬성이 굳이 담보되지 않아도 동시성을 충족시키는 것이 가능함.
  - 한 번의 하나의 작업만 처리할 수 있는 상황에서 여러 프로세스 간의 잦은 문맥교환을 통해 동시성을 만족시키면, 결과적으로 모든 프로세스가 동시에 동일하게 동작하는 것 같이 보이지만 실제로 병렬 처리된 것은 아닌 것

  (*A concurrent system supports more than one task by allowing all the tasks to make progress. In contrast, a paralel system can perform more than one task simultaneously.*)

​    

### 2-1. Programming Challenges

>  멀티 코어 시스템 환경에서 직면하게 되는 5가지 문제들

<img src="https://user-images.githubusercontent.com/68586291/152369336-f180a533-2706-4aea-80cf-e4c67777830f.png" alt="image" style="zoom:85%"/>

-  __작업의 정의(Identifying tasks)__ : 여러 독립적인 작업들을 병렬로 처리하고자 할 때, 어플리케이션의 작업 전체를 어떤 기준으로 어떻게 동시적인 작업 단위로 분할할 것인 지를 정의해야 함

- __균형(Balance)__  : 분리된 개별 작업들은 동일한 값의 동등한 양의 작업을 수행하는 구조여야 함

  (*Programmers must also ensure that the tasks perform equal work of equal value.*)

- __데이터 분할(Data splitting)__ : 어플리케이션의 전체 작업이 개별 작업 단위로 나뉘면, 나뉜 작업들이 접근하고 다루는 데이터 또한 각각의 코어에서 다룰 수 있도록 나뉘어야 함
- __데이터 의존성(Data dependency)__ : 분할된 작업들에 의해 다뤄지는 데이터는, 이것이 __두 개이상의 작업에 의존적인 지를 반드시 고려해야 하며,__ 하나의 작업이 다른 작업이 다루는 데이터에 접근할 경우에는 작업들 간의 순를 명확히 함으로써(__`synchronized`__) __데이터의 상태가 항상 최신을 유지할 수 있어야 함__
- __테스트 및 디버깅(Testing and Debugging)__ : 싱글 스레드보다 멀티 스레드 환경에서의 디버깅이 훨씬 까다롭고 어려움

​    

### 2-2. Types of Parallelism

> 데이터 측면의 병렬성과, 작업 측면의 병렬성에 대해 각각 알아보도록 한다.

<img src="https://user-images.githubusercontent.com/68586291/152370173-1acbc1e1-0b04-4975-a725-c9b57afdf401.png" alt="image" style="zoom:82%"/>

#### Data parallelism

- 각각의 코어에 동일한 데이터를 배분하여 모두 같은 작업을 수행하는 개념

  (*Distributing subsets of the same data across multiple computing cores and performing the same oepration on each core.*)

- 예를 들어 1부터 10까지의 합계를 구하는 작업을 수행하고자 할 때, 싱글 스레드라면 반복문을 통해 1부터 10까지 차례대로 더하면 되지만 2개의 스레드로 이를 병렬처리하면 하나는 1부터 5까지, 나머지는 6부터 10까지 더한 후 합하면 됨

#### Task parallelism

- 데이터가 아닌 스레드로 대표되는 작업 자체를 코어에 배분하여 작업을 수행하는 개념으로 각각의 스레드는 독자적인 작업(unique operation)을 수행함

  (*Distributing not data but tasks(threads) across multiple computing cores*)

- 위와 달리 각기 다른 스레드가 동일한 데이터에 접근하는 상황도 포함함

​    

## 3. Multithreading Models

<img src="https://user-images.githubusercontent.com/68586291/152375035-b6fd30a1-f195-4723-a321-20fa9a0d0441.png" alt="image" style="zoom:80%"/>

- 운영체제에서 스레드에 대한 지원은 유저 수준과 커널 수준으로 나눠볼 수 있음
- 유저 스레드는 커널 상위 수준에서 관리되며 커널의 개입이 없는 반면, 커널 스레드는 운영체제에 의해 직접 관리됨
- 유저 스레드와 커널 스레드 간의 관계를 크게 3가지 모델로 나눠볼 수 있음

​    

### 3-1. Many-to-One Model

<img src="https://user-images.githubusercontent.com/68586291/152376253-6367e26c-2673-4eae-8bce-c9d9bbd6701a.png" alt="image" style="zoom:80%"/>

- 여러 개의 유저 스레드가 하나의 커널 스레드와 관계를 맺는 구조
- 스레드의 관리는 유저 공간에 위치한 스레드 라이브러리에 의해 수행되며, 커널에의 접근이 없기 때문에 비용상 효율적임
- 하지만 특정 스레드가 작업을 차단하는 시스템 호출을 일으킬 경우에는 전체 프로세스가 멈추게될 수도 있음
- 커널 스레드에는 한 번에 하나의 유저 스레드만 접근할 수 있으며, 이로 인해 멀티 코어 환경에서 병렬적으로 여러 스레드의 작업을 수행하는 것이 불가능함
- 이로 인해 멀티 코어 시스템의 이점을 살릴 수 없기 때문에 잘 사용되지 않는 모델임

​    

### 3-2. One-to-One Model

<img src="https://user-images.githubusercontent.com/68586291/152378135-fce742ec-00a1-456c-baa9-1c62dba808e0.png" alt="image" style="zoom:80%"/>

- 여러 유저 스레드가 각각 하나의 커널 스레드에 일일히 대응되는 구조
- One-to-One과 비교했을 때, 하나의 스레드가 작업을 차단해도 다른 스레드가 작업을 계속해서 수행할 수 있기 때문에 동시성을 더욱 보장할 수 있음
- 단, 하나의 유저 스레드를 생성할 때마다 이에 대응되는 커널 스레드를 일일히 만들어야 하는 퍼포먼스적인 부담이 존재함
- 리눅스 및 윈도우 계열의 여러 운영체제에서 One-to-One을 사용하고 있음

​    

### 3-3. Many-to-Many Model

<img src="https://user-images.githubusercontent.com/68586291/152378251-1aa1bb3a-dbcd-4cf6-b984-291dba51f7b6.png" alt="image" style="zoom: 80%"/>

- 여러 유저 스레드가 전체 유저 스레드의 양보다 같거나 작은 규모의 커널 스레드와 관계를 맺는 구조
- 커널 스레드의 수는 특정 기계나 어플리케이션 환경에 따라 상이할 수 있음
- One-to-One과 비교했을 때, 동시성을 보장하면서 더 적은 양의 커널 스레드를 생성하면 되는 이점이 있음
- 이를 변형하여 전체적으로는 Many-to-Many의 구조를 유지하면서 일부 One-to-One의 형식을 섞은  __`two-level model`__ 도 존재함
- 시스템의 향상으로 인해 코어의 개수가 많아지면서 커널 스레드의 수를 제한하는 것이 점점 덜 중요해지기 때문에 One-to-One이 사실상 더 많이 쓰이고 있음

<br>

## Reference

- Abraham Silberschatz, Greg Gagne, Peter B. Galvin *__Operating System Concepts__* , Wiley
- [위키 피디아 병렬 컴퓨팅](https://ko.wikipedia.org/wiki/%EB%B3%91%EB%A0%AC_%EC%BB%B4%ED%93%A8%ED%8C%85)
- [멀티 코어 프로세서란? - 동커벨](https://donghoson.tistory.com/21)
- [동시성과 병렬 처리의 차이점](https://ko.gadget-info.com/difference-between-concurrency)
