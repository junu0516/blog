

+++
title = "Operating System Concepts - Chapter 2 요약"
tags = ["운영체제","CS스터디"]
categories = ["운영체제"]
date = "2021-10-23"

+++

공룡책 Ch 2 요약 (원서를 읽고 요약하는 과정에서 잘못된 내용이 있을 수 있습니다.)

![img](https://media.wiley.com/product_data/coverImage300/66/11198003/1119800366.jpg)

<br>

## 1. Operating-System Services

>  운영체제가 제공하는 서비스는 크게 아래와 같이 나눠볼 수 있음

- __유저 인터페이스__ : GUI, 터치스크린 등 유저친화적인 사용환경을 제공
- __프로그램 실행(Program Execution)__ : 프로그램을 메모리에 적재한 후, 프로세스를 실행하는 것
- __입출력(I/O Operation)__ : 입출력장치를 비롯하여 여러 입출력 관련 프로그램 실행과 관련
- __파일 시스템 조작(File-system manipulation)__ : 프로그램일 읽고 쓸 수 있는 파일과 디렉토리 관리를 위한 시스템을 제공
- __Communications__ : 공유 메모리( __`Shared Memory`__ ) 혹은 메시지 교환( __`Message Passing`__ ) 등을 통한 프로세스들 간의 정보 교환을 가능하게 하는 것
- __오류 감지(Error detection)__ : 프로그램이 실행되면서 발생할 수 있는 여러 오류를 감지하고, 해결

- __자원 할당__ : 여러 프로세스를 동시에 실행시켜야 하는 경우에,  CPU 등의 자원을 적절히 프로세스별로 분배시키는 것
- 이 외에도 로깅, 보안 등의 여러 기능을 수행

<br>

## 2. User and Operating-System Interface

### 2-1. Command Interpreters

- __명령어 인터프리터__ 라고도 해석하며, 운영체제나 프로그래밍 언어의 환경 등에서 해석하는 입력된 명령어를 읽고 실행하는 컴퓨터 프로그램을 말함
  - 대표적으로 리눅스의 __`shell`__ 이 인터프리터 프로그램에 속하며, __Third-Party shell__ , __Free User-Written shell__ 등의 형태도 가능함
- 명령어 인터프리터 자체가 명령 수행을 위한 코드를 포함할 수도 있고, __`Unix`__ 와 같이 인터프리터가 명령어를 해석하는 것이 아니라 각 명령에 해당하는 파일을 찾아 메모리에 적재 & 실행하는 형식이 될 수도 있음
  - 후자의 경우 __`rm file.txt`__ 라는 리눅스 명령어를 사용한다고 치면, 인터프리터가 이를 해석하는 것이 아니라 파일 시스템에서 __`rm`__ 이라는 파일을 찾아 실행시키는 것

<br>

### 2-2. Graphical User Interface & Touch-Screen Interface

- 윈도우의 GUI 환경이 대표적이며, 여러 파일에 대응하는 아이콘과 이를 담는 폴더를 통해 프로그램을 실행
- 스마트폰이나 태블릿과 같은 기기라면 여기서 더 나아가 터치스크린 기반의 인터페이스를 사용

<br>

## 3. System Calls

<img src="https://user-images.githubusercontent.com/68586291/138590591-08befe07-22e7-4e7e-bed1-76f34bceffbb.png" alt="image" style="zoom:120%;" />

- __시스템 호출__ 로 해석하며, 운영체제의 __`커널(kernel)`__ 이 제공하는 서비스에 대해, 응용 프로그램의 여러 요청에 따른 커널 접근을 위한 인터페이스를 말함
- 보통 C, C++과 같은 언어로 쓰여있으며, 일부 low-level task에 대해서는 어셈블리어로 쓰여있기도 함
  - __`어셈블리어(Assembly Language)`__ : 기계어와 1:1 대응이 되는 저급 언어로, 컴퓨터 구조에 따라 상이함

<br>

### 3-1. Application Programming Interface

- 간단한 프로그램이라 할 지라도, 순차적으로 명령어를 실행하기 위해 운영체제의 잦은 작업 및 시스템 호출을 수반함

- 개발자의 입장에서, 각 시스템 호출을 위한 디테일을 직접 보게될 일은 거의 없음

- 왜냐하면, __`API(Application Programming Interface)`__ 라고 불리는 인터페이스가 시스템 호출을 대신 수행하기 때문

  - 리눅스의 경우 C언어의 __`libc`__ 표준 라이브러리가 입출력 프로세스 및 메모리 할당 등 운영체제와 연관된 여러 작업 수행을 위한 함수를 제공

  *the functions that make up an API typically invoke the actual system calls on behalf of the application programmer*

- 같은 API를 지원하는 시스템 어디에서나 동일한 어플리케이션의 동작을 기대할 수 있으며, 실제 시스템 호출이 복잡할 경우에는 간단한 API 명령 호출로 이를 대체할 수 있음

  <img src="https://user-images.githubusercontent.com/68586291/138591002-783472e5-96fc-4f51-9a39-b4ed8066299a.png" alt="image" style="zoom:120%;" />

- __`Run-time Environment(RTE)`__ : 프로그램이 실행되고 있는 동안의 동작을 의미하며, 이를 위한 프로그래밍 언어, 컴파일러, 라이브러리 등의 개념을 모두 포함함

- 런타임 환경은 시스템 호출 인터페이스(__`System-call interface`__ )를 제공하며, 이는 운영체제로 하여금 시스템 호출을 가능하게 함

- 시스템 호출 인터페이스가 API의 명령 호출을 받아, 이를 수행하기 위한 운영체제의 시스템 호출을 부르는 것

  *The system-call interface intercepts function calls in the API and invokes the necessary system calls within the operating system.*

- 이런 상황에서 명령어를 호출하는 쪽, 즉 개발자는 구체적인 시스템 호출의 상세를 이해하기 보다는 API의 호출과, 호출의 결과가 운영체제의 어떤 동작을 수반하는 지 정도를 이해하면 됨

  - 운영체제 동작 과정 등의 상세한 것들은 대부분 프로그래머가 겉으로 봐서 알 수 없도록 숨겨져 있는 것

- 여러 시스템 호출이 순차적으로 일어나는 경우에는 각 과정에서의 수행 결과를 파라미터로서 넘기는 방법에 대해 고민해볼 수 있음

  - 제일 간단한 방법은 각 과정에 해당하는 레지스터로 파라미터를 넘겨주는 것
  - 하지만 레지스터의 양보다, 넘길 파라미터의 양이 더 많다면 이를 특정 메모리 공간에 저장하고, 주소값을 파라미터로 레지스터에 넘겨도 됨

  *The simplest appraoct is to pass the parameters in registers. In some cases, however, there may be more parameters than registers. In these caes, the parameters are generally stored in a block, or table, in memory, and the address of the block is passed as a parameter in a register.*

  <img src="https://user-images.githubusercontent.com/68586291/138592060-9797fd71-7607-4e88-9eac-5d87e94618c2.png" alt="image" style="zoom:130%;" />
  - 파라미터의 전달은 __`stack`__ 에서의 pop & push 과정을 통해 일어남

<br>

### 3-2. Types of System Calls

