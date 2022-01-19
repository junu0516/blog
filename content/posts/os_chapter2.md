

+++
title = "Operating System Concepts - Chapter 2 요약"
tags = ["운영체제"]
categories = ["Computer Science"]
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

<img src="https://user-images.githubusercontent.com/68586291/138590591-08befe07-22e7-4e7e-bed1-76f34bceffbb.png" alt="image" style="zoom:150%;" />

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

  <img src="https://user-images.githubusercontent.com/68586291/138591002-783472e5-96fc-4f51-9a39-b4ed8066299a.png" alt="image" style="zoom:150%;" />

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

  <img src="https://user-images.githubusercontent.com/68586291/138592060-9797fd71-7607-4e88-9eac-5d87e94618c2.png" alt="image" style="zoom:150%;" />
  
  - 파라미터의 전달은 __`stack`__ 에서의 pop & push 과정을 통해 일어남

<br>

### 3-2. Types of System Calls

- 시스템 호출은 크게 프로세스 컨트롤, 파일 관리, 장치 관리, 정보 저장, 커뮤니케이션, 보안의 6개 카테고리로 나눌 수 있음

1) __프로세스 컨트롤(Process Control)__

   - 프로세스를 실행하거나 종료하는 일을 수행함

     - 만일 프로그램 실행 중에 오류가 발생하면, 실행중이던 프로세스를 강제로 종료하도록 하는 시스템 호출이 일어나며 명령어 인터프리터(command interpreter)로 하여금 다음 명령을 읽어들여 수행하도록 유도하는 것
     - 때때로 오류가 발생하면 메모리 덤프(dump of memory)를 뜨고 디버거(debugger)에 의해 해당 에러에 대한 분석이 일어날 수도 있음
     - 윈도우와 같은 GUI 시스템에서는 에러 발생 시 시스템 호출과 함께 이에 대한 팝업(alert)이 사용자 화면에 나타남
     - 에러의 정도(Severity of error)를 구분하여 명령어 인터프리터로 하여금 자동으로 상황에 따른 에러 처리를 달리하도록할 수도 있음

   - 한 프로세스의 실행은, 다른 프로세스를 메모리에 적재하고 실행하도록 시스템 호출을 일으킬 수도 있음 ( 혹은 메모리를 해제할 수도 있음 ) 

     *A process executing one program may want to load() and execute() another program.*

     -  이러한 시스템 호출의 기능은 명령어 인터프리터로 하여금 사용자에 의해 특정 프로그램을 실행하거나 마우스 클릭 등과 같은 이벤트의 발생 시에 또 다른 프로그램을 실행하도록 유도하는 것을 의미함

     - 여기서 메모리에 새로 적재된 프로그램이 종료된 경우에는 기존에 존재하던 프로그램에 대한 제어권의 반환이 어디에 이루어져야 하는 지에 대한 의문이 있을 수 있음

       *An interesting question is where to return control when the loaded program terminates.*

     - 다시 말해 적재된 프로그램을 그대로 메모리에 삭제하거나 둘 지, 혹은 새로운 프로그램과 병행하여 실행상태로 계속 둘 지 선택해야 하는 것

       *If control returns to the existing program when the new program terminates, we must save the memory image of the existing program; thus, we have effectively created a mechanism for one program to call another program. If both progrmas continue concurrently, we have created a new process to be multiprogrammed. Often, there is a system call specifically for this purpose(create_process())*

   - 두 개 이상의 프로세스는 특정 데이터를 공유할 수도 있는데, 여 기서 데이터의 무결성(integrity of shared data) 보존을 위해 운영체제는 데이터에 __`lock`__ 을 거는 시스템 호출을 일으킬 수 있음

     - 다시 말해 공유 자원에 하나의 프로세스가 접근 중일 동안에는, 락이 걸려있기 때문에 이것이 해제될 때 까지 다른 프로세스가 접근할 수 없는 것

   - 이 외에도 특정 이벤트를 기다리거나(wait event) 신호를 알리는 이벤트(signal event)의 발생을 유도할 수도 있을 것

2) __파일 관리(File Management)__

   - 파일 혹은 디렉토리에 대한 생성과 삭제, 읽기와 쓰기 등의 행위 제어에 관한 것

3) __장치 관리(Device Management)__

   - 장치는 물리 장치(physical devices)와 가상 장치(virtual devices) 모두를 포함하는 개념이며, 운영체제에 의해 관리되는 자원(resources)을 여기서는 장치라고 표현
     - 다시 말해, 장치 관리는 곧 프로세스 수행에 필요한 자원의 관리를 일컫는 것
   - 프로세스 수행에 필요한 자원이 가용 상태일 때 비로소 해당 프로세스에 제어권이 오며, 그렇지 않다면 프로세스는 자원이 가용상태일 때 까지 대기해야 함
   - 자원의 사용은 요청( __`request()`__ )을 통해 해당 프로세스에 의한 배타적인 사용권한을 부여하고, 이를 읽거나(__`read()`__) 쓰고(__`write()`__) 난 후 다시 반환( __`release()`__ )하는 일련의 과정을 거치게 됨

4) __정보 보존(Information Maintenance)__

   - 디버깅 등의 목적을 위해 가용 메모리 크기, 디스크 공간 혹은 프로그램 수행 시간 등의 정보를 시스템 호출을 통해 반환하는 것을 의미
   - 특히 많은 운영체제는 프로그램이 수행되는 위치와 수행 시간 등의 정보를 제공하는 기능을 수행하는데, 타이머 인터럽트(__`timer interrupts`__)의 발생시켜서 측정하는 것
   - 운영체제는 프로세스 자체에 대한 정보를 보관할 수도 있는데, 여기서 시스템 호출은 운영체제가 보관하는 프로세스 정보 접근을 위해서도 사용됨

5) __커뮤니케이션(Communication)__

   - 커뮤니케이션은 크게 __message-passing model__ 과 __shared-memory model__ 의 두 가지 형태로 나뉨

   - Message-Passing Model

     - 프로세스 간에 서로 메시지를 주고받는 식으로 소통하는 형태로 서버-클라이언트 간 메시지 전달 형태와 유사함
     - 커넥션을 열고 닫거나, 메시지를 읽고 쓰는 등의 각각의 행위에 대한 시스템 호출이 발생하는 것

   - Shared-Memory Model

     - 시스템 호출을 통해 두개 이상의 프로세스가 공통된 메모리 영역에 접근하는 방식

     - 앞서 언급한, 운영체제가 특정 공유 데이터에 대한 락을 거는 등의 제한을 없애고 공유된 영역에서 프로세스들이 데이터를 읽고 씀

     - 단, 여러 프로세스가 동시에 쓰기 행위를 하는 일이 발생하지 않아야 하는 책임이 존재

       *Shared memory requires that two or more processes agree to remove this restriction. (...) The processes are also responsible for ensuring that they are not writing to the same location simultaneously.*

6) __보안 혹은 보호(Protection)__

   - 컴퓨터 시스템이 제공하는 자원에 대한 접근을 제어하는 매커니즘 제공을 의미함
   - 특정 자원에 대한 허가권을 부여하거나, 유저의 접근을 허가하는 행위 등에 대한 시스템 호출이 있을 수 있음

<br>

## 4. System Services

- System Service는 System Utilities라고도 불리며, 프로그램 개발 및 실행에 편리한 환경을 제공해주는 것을 일컫으며, 단순한 시스템 호출을 위한 사용자 인터페이스 또한 시스템 서비스의 일종이라 할 수 있음

### 4-1. System Service Categories

- **File Management** : 파일 혹은 디렉토리의 생성, 읽기, 수정, 복사 등의 행위에 관한 것들
- **Status information** : 레지스트리와 같은 설정 및 상태 정보를 저장하고 운용하는 것들
- **File modificatio** : md, properties 등의 확장자를 지닌 파일을 메모장과 같은 에디터 프로그램을 통해 수정하는 것을 생각하면 됨
- **Programming-language support** : 일반적인 프로그래밍 언어를 위한 컴파일러나 인터프리터, 혹은 디버거 등을 운영체제가 제공하기도 함
- **Program loading and execution** : 프로그램의 실행을 위해, 정적인 코드 덩어리 상태의 프로그램을 메모리에 올려 실행 가능한 상태로 하는 것
- **Communications** : 프로세스들, 유저들, 혹은 다른 컴퓨터 시스템들 간의 가상 연결 등과 같은 매커니즘을 제공하는 프로그램들
- **Background services** : 일반적이고 보편적인 목적의 시스템을 백그라운드에서 돌리는 경우

*Along with system programs, most operating systems are supplied with programs that are useful in solving common problems or performing common operations. (...) The view of the operating system seen by most users is defined by the application and system programs, rather than by the actual system calls.*

<br>

## 5. Linkers and Loaders

<img src="https://user-images.githubusercontent.com/68586291/149954302-7ea88267-6ae1-4409-8771-b07073789f6f.png" alt="image" style="zoom:80%">

- 보통 CPU 상에서 프로그램을 돌리고자 한다면, 프로그램을 메모리에 올려 실행 가능한 프로세스 상태가 되도록 해야 함

- 여러 프로그래밍 언어로 된 소스 파일(__`source files`__)들이 컴파일 되어 목적 파일(__`object file`__)이 되며, 이것이 물리적인 메모리 위치에 로드되는 것

  (*source files are compiled into object files that are designed to be loaded into any physical memory location, a format known as relocatable object file*)

- 이후 링커(__`linker`__) 라고 불리는 것이 이러한 목적 파일들을 합쳐 하나의 실행 가능한 바이너리 파일(__`single binary executable file`__) 로 만들어줌
  - 링커가 작용하는 과정에서 여러 목적 파일 혹은 라이브러리가 하나로 합쳐짐

    (*During the linking phase, other object files or libraries may be included as well, such as the standard C or math library*)

  - 가령 자바 파일이 컴파일된 경우라면, java.util 라이브러리와 같이 자동으로 제공되는 것들이 사용자에 의해 만들어진 파일과 합쳐는 경우를 생각하면 됨

- 로더(__`loader`__)는 이러한 바이너리 파일을 메모리에 올리는 역할을 수행하며, 이후 CPU 자원을 사용하여 실행 가능한 상태가 되는 됨

- 링커와 로더가 작용하는 과정을 __`relocation`__ 이라고도 표현하며, 프로그램의 실행을 위한 모든 코드 및 기타 라이브러리 등의 데이터가 하나로 합쳐져 프로그램의 최종적인 주소값이 할당되는 것을 의미함

- 따라서 프로세스(__`process`__) 는 이러한 실행 가능한 파일에 필요한 라이브러리들이 연결되있고 이것이 메모리에 적재된 것으로 표현할 수 있음

  - 대부분의 시스템은 프로그램이 메모리에 로드되면 동적으로 라이브러리를 연결시키도록 되어있음

  - 윈도우를 예로 들면, DLL 형식의 파일을 지원하는데 이는 런타임 시점에 필요한 경우에만 라이브러리가 실행파일의 일부분이 된다는 이점이 있음

    (*The benefit of this approach is that it avoids linking and loading libraries that may end up not being used into an executable file. Instead, the library is conditionally linked and is loaded if its is required during program run time*)

<br>

## 6. Why Applications Are Operating-System Specific

- 일반적으로 하나의 운영체제 하에서 컴파일된 어플리케이션은 다른 운영체제 환경에서 실행 불가능함
- 여러 운영체제들이 각자의 시스템 호출을 가지거나 어플리케이션의 바이너리 형식을 읽어들이는 방식이 다르는 등  환경이 상이하기 때문
- 만일 복수의 운영체제 환경에서 실행가능한 파일이라면 아래의 이유로 인한 것임을 유추해볼 수 있음

***

1. 어플리케이션이 파이썬이나 루비같은 인터프리터 언어로 쓰여진 경우
   - 인터프리터가 기본적으로 복수의 운영체제에서 사용 가능하며, 인터프리터가 코드를 읽어들이면서 현재 운영체제에 알맞은 시스템 호출을 일으키는 것
   - 단, 환경마다 퍼포먼스가 상이할 수 있다는 점을 고려해야 함
2. 어플리케이션이 자바와 같이 가상머신(자바의 경우 __`JVM`__) 환경에서 실행되는 언어로 쓰여진 경우
   - 자바의 경우 런타임 환경에 로더(__`lodaer`__)와 바이트 코드 검증기(__`byte-code verifier`__) 및 자바 가상 머신에 자바 어플리케이션을 올리는 기타 다른 요소 등이 포함되어 있음
   - 이러한 런타임 환경이 위에서의 인터프리터와 유사하게 자바 어플리케이션이 복수의 운영체제 환경에서 실행되는 것을 가능하게 하는 것
3. 이 외에 개발자가 복수의 운영체제 환경을 염두에 두고 각 환경에서 실행 가능한 코드를 작성하는 것

***

<br>

## 7. Operating-System Structure

- 운영체제 구조의 제일 기본적인 접근은 커널이 모든 것을 포함하는 것이 아닌, 전체를 여러 모듈로 나눠 접근하는 방식임
- 코드로 치면 모든 로직이 __`main()`__ 안에 있는 것이 아니라, 모듈화를 한 후 여러 함수를 __`main()`__ 에서 필요에 따라 조합하여 호출하는 방식인 것
- 여러 부분이 커널에 합쳐지는 방식에는 여러 가지가 있음

​    

### 7-1. Monolithic Structure

<img src="https://user-images.githubusercontent.com/68586291/150074426-a46be681-a57e-47ad-bf58-b8424e12ca9a.png" alt="image" style="zoom:70%"/>

- 커널은 일반적으로 시스템 호출을 통해 파일 시스템, CPU 스케쥴링, 메모리 관리 등의 기능을 수행함
- 모놀리식 구조는 단일형 구조를 의미하며, 제일 간단한 접근 방식으로 모듈화 없이 운영체제의 기능이 단일한 메모리 공간 위에서 동작하도록 하는 방식임
  - 오리지널 유닉스 구조가 이런 방식이며, 크게 커널과 시스템 프로그램의 두 부분으로 나뉜 형태였음
  - 이러한 구조였던 것이 유닉스가 진화하면서 커널이 여러 디바이스 드라이버와 인터페이스로 나뉘어 추가 및 확장되온 것
- 구조상으로는 단일한 것이 제일 간단하지만 기능을 확장시키는 것이 힘들다는 단점이 있음

​    

### 7-2. Layered Approach

<img src="https://user-images.githubusercontent.com/68586291/150077110-4d3ca11e-a91a-487b-802a-de09b33593ee.png" alt="image" style="zoom:80%">

- 단일형 구조가 tightly-coupled한 특징이 있다면, 전체를 부분으로 나누는 모듈화 구조는 loosely-coupled한 특징이 있음

  - 각각의 고유한 기능을 가진 부분으로 전체를 나누어 접근하는 방식이며, 각각의 모듈에 변화가 있더라도 다른 모듈에는 영향을 미치지 않음

- 이러한 모듈화를 구현하는 대표적인 방식이 계층화로, 보통 제일 낮은 계층이 하드웨어이고 제일 높은 계층이 유저 인터페이스에 해당함

- 운영 체제 계층은 데이터와 데이터를 조작하는 작업으로 구성된 추상 객체의 구현임

  (*An operating-system layer is an implementation of an abstract object amde up of data and the operations that can manipulate those data*)

- 계층화된 접근의 제일 주요한 장점은 모듈화가 간단하다는 점과, 디버깅하기 유리하다는 점임

  - 모듈이 순차적으로 계층화되있기 때문에, 특정 기능에 문제가 있으면 계층별로 서로 독립적이기 때문에 하위부터 상위로 순서대로 살펴보면 되기 때문
  - 이러한 계층화 구조가 제일 많이 적용된 것이 __`TCP/IP`__ 등과 같은  네트워크 프로토콜 혹은 이를 적용한 웹 어플리케이션임

- 하지만 이러한 장점에도 불구하고 기능별로 계층을 구분하는 것의 어려움으로 인해 소수의 운영 체제만이 완전한 계층화를 채택하고 있음

​    

### 7-3. Microkernels

<img src="https://user-images.githubusercontent.com/68586291/150077270-63d3798b-f4ac-4d65-9da3-7170dbafba3c.png" alt="image" style="zoom:80%"/>

- 단일 구조에서 언급했듯, 유닉스가 진화하면서 커널의 규모가 점점 크고 다루기가 어려워짐에 따라 단일 구조로 관리하기가 어려워졌음
- 1980년대 중반 카네기 멜론 대학의 연구자들은 __`Mach`__ 이라고 불리는 운영체제를 개발했는데, 이는 마이크로 커널이라 불리는 모듈화된 커널을 적용한 운영체제였음
- 이는 운영체제에서 필수적이지 않는 부분을 커널에서 모두 제거해서 별도의 유저 레벨의 메모리 공간에서 커널과 함께 동작하도록 한 방식임
- 따라서, 커널의 규모는 결과적으로 더 작아졌기 때문에 마이크로 커널이라 불리게 된 것이며, 마이크로 커널은 커뮤니케이션과 최소한의 프로세스와 메모리 관리만을 제공함
  - 여기서 __커뮤니케이션__ 은 마이크로 커널이 클라이언트 프로그램과 여러 서비스들 간에 메시지 전달(__`Message Passing`__)을 통해 이루어지는 것을 의미
  - 예를 들어 클라이언트 프로그램이 특정 파일에 접근하고자 한다면, 파일 서버와 프로그램 간에 서로 이를 위한 메시지를 주고받게 되는 것
- 이러한 마이크로커널 방식은 우선 계층화와 마찬가지로 운영체제의 규모 확장을 더욱 쉽게 한다는 이점이 있음.
  - 모든 새로운 서비스는 유저 공간에 추가되며 여기서 커널의 수정은 필요 없기 때문
  - 또한 커널의 수정이 필요한 경우에 수정해야 될 것들의 양도 상대적으로 적음
- 또한 대부분의 서비스가 유저 레벨에서 동작하기 대문에 커널 자체의 보안 및 신뢰성의 제고라는 이점이 있음
- 하지만 이러한 마이크로커널 방식은 __`system-function`__ 의 오버헤드 증가로 인한 퍼포먼스 문제가 존재한다는 한계 또한 있음
  - 예를 들어 2개의 유저 레벨 서비스가 서로 커뮤니케이션할 경우 서로 다른 메모리 공간 사이에 복수의 메시지가 전달되는데 마이크로커널의 경우 대부분의 기능이 유저 레벨에 존재하기 때문에 오버헤드가 당연히 많을 수 밖에 없는 것
  - 이는 꽤나 중요한 문제라 윈도우의 경우 초기 버전에서 진화할수록 점점 마이크로커널이 아닌 모놀리식 방식으로 변모하게 되었음

​    

### 7-4. Modules

- 위에서 언급된 각 방식의 단점을 종합하여 오늘날 제일 많이 사용되는 것은 __`lodable kernel modules(LKMs)`__ 라 불리는, 동적으로 커널 모듈이 추가되는 방식임

- 이는 간단히 말해 커널은 핵심 기능만 수행하고 나머지는 동적으로 모듈이 추가되는 것을 의미함

- 커널이 동작하고 있으면 링킹 시스템이 필요에 따라 동적으로 새로운 기능을 커널에 직접 추가하며, 커널이 변경될 때마다 컴파일을 다시 하는 방식임

- 이는 커널이 섹션별로 여러 인터페이스로 분리된다는 점에서 계층화와 비슷해 보이지만, 여러 모듈이 아무나 서로 호출할 수 있다는 점에서 더욱 융통성 있음

  (*The overall result resembles a layered system in that each kernel section has defined, protected interfaces; but it is more flexible than a layered system, because any module can call any other modules*)

- 또한 주요 모듈이 주 기능을 가진 상태에서 다른 모듈과 상호작용하기 때문에 마이크로커널과 유사해 보이지만, 메시지 전달(__`Message Passing`__) 과정을 수반하지 않기 때문에 더욱 효율적임

  (*The approach is also similar to the microkernel approach in that the primary module has only core functions and knowledge of how to load and communicate with other modules; but it is more efficient, because modules do not need to invoke message passing in order to communicate.*)

- 리눅스가 LKMs를 적용한 대표적인 운영체제임

<br>

## Reference

- Abraham Silberschatz, Greg Gagne, Peter B. Galvin *__Operating System Concepts__* , Wiley
