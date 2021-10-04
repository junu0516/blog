

+++
title = "Operating System Concepts - Chapter 1 요약"
tags = ["운영체제","CS스터디"]
categories = ["운영체제"]
date = "2021-09-21"

+++

공룡책 Ch 1 요약 (원서를 읽고 요약하는 과정에서 잘못된 내용이 있을 수 있습니다.)

![img](https://media.wiley.com/product_data/coverImage300/66/11198003/1119800366.jpg)

​    

## 1. What Operating Systems Do

- 운영체제의 간단한 정의 : __컴퓨터의 하드웨어를 매니징하는 소프트웨어__
- 컴퓨터 하드웨어와 사용자 간의 매개체로 기능
- 운영체제에 대해 자세히 알기 전에, 컴퓨터 하드웨어의 구조에 대해서 살펴볼 필요가 있음
- __Hardware__ : CPU, 메모리, 입출력 장치로 크게 나뉘며 기본적인 컴퓨터 자원을 제공하는 역할을 수행
- __Application Programs__ : 프로세서, 스프레드시트, 컴파일러 등의 소프트웨어를 일컫으며 할당받은 컴퓨터 자원이 유저에 의해 어떻게 소비되는 지를 나타냄

​    

![1](https://user-images.githubusercontent.com/68586291/134378969-ceda5c2f-2e89-4435-bba8-d8a0a4238521.PNG)

### 1-1. User View

- 사용자 관점에서 운영체제는 __ease of use__ 의 목적을 위해 존재
- 퍼포먼스 및 보안 향상, 자원의 활용 등의 관점에서 운영체제에 대해 고민해볼 수 있음

### 1-2. System View

- 자원 할당자 측면에서의 운영체제를 이야기하는 것(__Resource Allocator__)
- CPU time, 메모리 공간, 입출력장치 등이 자원에 해당하며 운영체제는 이들 자원을 적절히 분배하여 프로세스에 할당하는 역할을 수행하는 것
- __`Control Program`__ 관점에서의 운영체제를 생각할 경우, 운영체제가 프로그램의 수행 및 여러 에러 방지 등의 역할을 수행하는 것으로도 볼 수 있음

### 1-3. Defining Operating Systems

- 여러 관점을 고려해야 하기 때문에 100% 완벽하게 운영체제에 대해서 무엇이다라고 정의하기는 힘듦
- 여러 프로그램들이 실행을 위해 자원을 할당받아야 하는 공통점이 있기 때문에, 자원의 할당 및 컨트롤 측면에서 운영체제를 정의하는 것이 제일 보편적임
- __`kernel`__ (커널) 이라 불리는 프로그램으로 운영체제를 생각해볼 수 있으며, 이러한 관점에서 프로그램은 크게 __`System Program`__ 과 __`Application Program`__ 의 두 가지로 나눠볼 수 있음
- __System Program__ : 운영체제의 동작과 연관되있지만 커널의 일부는 아닌 프로그램을 말함
- __Application Program__ : 시스템 프로그램을 제외한 나머지 프로그램들을 말함

<br>

​    

## 2. Computer-System Organization

![2](https://user-images.githubusercontent.com/68586291/134381350-def6d3ee-6e71-45be-a362-815c6bce5332.png)

- 보편적인 컴퓨터 구조는 __`버스 구조`__ 상에 여러 디바이스 컨트롤러와 CPU가 메모리와 연결된 구조를 띄고 있음

  > System Bus : CPU와 디바이스 컨트롤러, 메모리를 Bus 구조로 연결하는 Subsystem을 일컫는 용어

- 디바이스 컨트롤러(Device Controller)는 장치로부터 들어오가 나가는 데이터를 임시로 저장하기 위한  __`로컬 버퍼(Local Buffer)`__ 와 __`레지스터(Register)`__  를 각각 가지고 있음

- CPU는 각각의 디바이스 컨트롤러와 소통하기 위해, 각각에 대응하는 __`디바이스 드라이버(Device Driver)`__ 를 가지고 있음

- 디바이스 드라이버는 운영체제로 하여금 디바이스 컨트롤러의 동작을 이해할 수 있도록 하며,  운영 체제에 해당 장치에 대한 동일한 인터페이스(Uniform Interface)를 제공함

  *(provides the rest of the operating system with a uniform interface to the device.)*

<br>

​    

### 2-1. Interrupts

![3](https://user-images.githubusercontent.com/68586291/134777879-7838d9af-f3de-410c-bb87-3d4f1f2bfd26.PNG)

- 단순한 컴퓨터에서의 입출력 과정을 살펴보도록 하자

  - 어떤 프로그램이 입출력을 수행한다고 할 경우, 디바이스 드라이버는 우선 디바이스 컨트롤러에 입출력을 위해 적절한 레지스터(Register)를 적재(load)하게 될 것임

  - 디바이스 컨트롤러는 데이터를 읽어들여 로컬 버퍼로 데이터를 다시 이동시킨 후 디바디스 드라이버에 데이터의 이동이 끝났음을 알림

  - 이후 디바이스 드라이버는 운영체제의 다른 부분에 읽어들인 데이터 혹은 데이터의 포인터값이나 상태 정보(status information) 등을  리턴하여 운영체제가 이후의 행위를 수행하도록 함

    (*Device driver gives control to other parts of the operating system, possibly returning the data or a pointer to the data. For other operations, the device driver returns status information ...*)

- 여기서 __`인터럽트(Interrupt)`__ 는 CPU가 프로그램을 실행하고 있을 때 시스템 버스 경로를 따라 CPU에 특정 신호(Signal)를 보내 하드웨어 장치에 특정 수행을 완료했거나 예외 상황의 발생 혹은 특정 처리가 필요한 상황 등을 알리는 역할을 수행함

  > 엄밀히는, 하드웨어와 소프트웨어 측면의 인터럽트로 나뉘지만 여기서는 주로 하드웨어 측면의 인터럽트에 대해 언급하도록 함

- 인터럽트가 수행중이면 CPU는 수행중이던 동작을 멈추고, 수행중이던 프로세스 정보를 PCB(프로세스 제어블록)에 저장하게 됨

  (*When CPU is interrupted, it stops what it's doing and immediately transfers execution to a fixed location.*)

- __`인터럽트 핸들러(Interrupt Hnadler)`__ 는 실제 인터럽트를 처리하기 위한 일종의 루틴(Routine)으로, 운영체제의 코드 영역에는 여러 인터럽트 유형별로 어떻게 처리해야 할 지를 프로그램화해서 저장하고 있음

  - __Routine__ 이라는 단어의 뜻에서 알 수 있듯, 인터럽트 요청에 따른 주기적인 수행이 정해져 있으며 다른 말로 __`인터럽트 서비스 루틴(Interrupt Service Routine)`__ 이라고도 부름

- 여기서 각각의 인터럽트 핸들러들의 주소를 인터럽트별로 보관하게 되는데, 이를 저장하는 자료 구조를 __`인터럽트 벡터(Interrupt Vector)`__ 라고 부름

- 문제는, 인터럽트 벡터가 수용할 수 있는 범위 이상으로 컴퓨터에 연결된 디바이스가 많기 때문에 너무 많은 인터럽트가 동시에 발생하게 될 경우에는 골치아파짐

  - __`인터럽트 체이닝(Interrupt Chaining)`__ 이 이를 위한 대표적인 방안으로, 인터럽트 벡터의 각 요소가 인터럽트 핸들러 리스트를 가리키도록 하는 방식임

  - 인터럽트가 발생하면 해당 인터럽트에 대한 벡터 내 요소가 가리키는 리스트가 호출되고, 리스트의 각 인터럽트 핸들러를 하나씩 보면서 적절한 핸들러를 찾게되는 것

    (*...interrupt chaining, in which each element in the interrupt vector points to the head of a list of interrupt handlers. When an interrupt is raised, the handlers on the corresponding list are called one by one, until one is found tha can service the request.*)

    <br>

- __`인터럽트 우선순위(Interrupt Priority Levels)`__ 를 정해놓기도 하는데, 이는 동시다발적인 인터럽트 발생 시 CPU로 하여금 전원공급 이상이나 CPU의 기계적 오류 등의 높은 우선순위를 가지는 인터럽트를 먼저 처리할 수 있도록 하기 위함

  <br>

​    

### 2-2.  Storage Structure

![4](https://user-images.githubusercontent.com/68586291/134811418-480522ec-d31e-44c8-8d0a-6fb120fcaaee.PNG)

- 여기서는 저장 장치의 구조를 크게 __Primary, Secondary, Tertiary__ 의 세 가지로 나눠서 살펴볼 것이며, 각각의 구조는 속도와 크기, 휘발성 여부에 차이가 있음

  (The main differences among the various storage systems lie in speed, size, and volatility.)

- CPU가 특정 작업을 수행하기 위해서는, 작업 수행 정보를 반드시 메모리에 적재해야 함

- 또한, 컴퓨터에서 프로그램이 돌아가기 위해서는 프로그램 코드가 메모리에 적재되어 있어야 함

- 컴퓨터는 대부분의 프로그램을 메인메모리( __`RAM, Random-Access Memory`__ )상에서 실행하는데 이러한 메인 메모리의 특성은 __휘발성(Volatile)이 존재하기 때문에__ 컴퓨터가 종료되면 데이터를 모두 잃게 됨

- 이를 극복하기 위해서 사용되는 것이 __`EEPROM(Electrically Erasable Programmable Read-Only Memory)`__ 이며, 데이터를 바이트 단위로 읽고(Read) 쓸 수 있는(Write) 비휘발성 메모리란 특징이 있음

  - EEPROM은 RAM에 비해 상대적으로 속도가 느리기 때문에 주로 자주 쓰이지 않는 정적인 프로그램이나 데이터를 저장하는 용도로 쓰임
  - 예를들어 아이폰의 경우 EEPROM에 시리얼 넘버나 하드웨어 정보와 같은 것들을 저장해놓고 있음

- 모든 종류의 메모리는 바이트의 배열 형식의 데이터를 담고 있으며, 각각의 바이트는 고유의 주소값을 가짐

  - 여기서 메모리에의 적재(loading) 과정은, 메인 메모리에서 바이트 기반 데이터들을 CPU 내부의 레지스터(Register)로 옮기는 것을 말하며, 반대로 저장(storing) 과정은 적재와 반대로 데이터를 레지스터에서 메인메모리로 옮기는 과정을 의미함

    *(The load instruction moves a byte or word from main memory to an internal register within the CPU, whereas the store instruction moves the content of a register to main memory.)*

- __`명령 주기(Execution Cycle)`__ 는 CPU가 메모리로부터 특정 명령을 가져와 이를 통해 어떤 동작을 요구할 지 결정하고, 이를 수행하는 과정을 의미함

  - 맨 처음 명령(instruction) 을 메모리로부터 가져온 후, CPU 내부의 레지스터(instruction register)에 저장함

  - 저장된 명령은 복호화되어 수행(execution)을 위한 피연산자(operand)를 메모리에서 가져오도록 함

  - 피연산자에 의한 명령 수행이 완료되고 나면, 수행의 결과 역시 다시 메모리에 저장됨

    *(...first fetches an instruction from memory and stores that instruction in the instruction register. The instruction is then decoded and may cause operands to be fetched from memory and stored in some internal register. After the instruction on the operands has been executed, the result may be stored back in memory.)*

- 하지만 앞서 언급한대로, 메인 메모리는 __휘발성__ 이라는 한계와 더불어, 많은 양의 프로그램이나 데이터를 저장하기에는 __공간 사이즈가 너무 작다는 문제가 있음__

- 따라서, EEPROM의 언급과 비슷한 맥락으로 대부분의 컴퓨터는 비휘발성의  __`Secondary-Storage`__ 를 갖추고 있음

  - 이것의 대표적인 예가 하드 디스크 드라이브( __`HDD`__ )와 비휘발성 메모리 디바이스( __`Nonvolatile Memory Devices`__ )이며 휘발성 메모리에 미해 상대적으로 느리지만 많은 양의 데이터를 저장할 수 있기 때문에 서로 보완관계에 있음

- 비휘발성 메모리는 Secondary 레벨 외에도, 더욱 하위 개념인 __`Tertiary Storage`__ 로도 나눠볼 수 있는데, CD-ROM이나 블루레이 등과 같은 특수 목적의 메모리 디바이스가 여기에 속함

<br>

​    

### 2-3. I/O Structure

![5](https://user-images.githubusercontent.com/68586291/135765438-e8e219a1-6dd8-4dba-bf60-bd9ce8997340.PNG)

- 위의 그림은 일반적인 인터럽트에 의한 I/O 처리 장치의 구조로 큰 규모의 데이터를 옮길 경우에는 높은 오버헤드를 유발하게 됨
- 이를 해결하기 위해 __`Direct Memory Access(DMA)`__ 가 사용됨
  - CPU에 독립적으로 메모리에 접근할 수 있도록 하는 시스템 기능으로, CPU를 거치지 않고 디바이스 컨트롤러가 큰 규모의 데이터를 직접 디바이스에서 메인 메모리로 옮기는 것
  - 이 경우에는 디바이스 드라이버에 해당 연산이 완료되었음을 알리기 위한 하나의 인터럽트만이 발생하게 됨
  - 따라서 디바이스 컨트롤러가 연산을 수행하는 동안 CPU는 다른 작업을 수행할 수 있게 되는 것

<br>

## 3. Computer-System Architecture

주요 개념을 아래와 같이 다시 정의해볼 수 있음

- __`CPU`__ : 명령을 수행하는 하드웨어 개념(중앙처리 장치)
- __`Processor`__  : 물리적인 칩으로, 하나 이상의 CPU를 포함하고 있음
- __`Core`__ : CPU의 기본적인 단위를 일컫음
- __`Multicore`__ : 복수의 CPU 단위를 가지고 있음을 의미
- __`Multiprocessor`__ : 복수의 프로세서를 가지고 있는 형태를 의미

<br>

### 3-1. Single-Processor Systems

- 원시적인 컴퓨터의 경우 하나의 CPU와 하나의 코어(Single Core)만을 가진 Single-Processor System 형태였음

- __`core`__ : 명령을 수행하는 단위로 데이터를 로컬에 저장하기 위한 레지스터이기도 함

  (*The core is the component that executes instructions and registers for storing data locally.*)

<br>

### 3-2. Multiprocessor Systems

- 오늘날 모바일 기기를 포함한 대부분의 컴퓨터는 멀티 프로세서 구조를 보임

- 일반적인 정의는 2개 이상의 프로세서를 가진 시스템을 말하며, 각각의 프로세스는 하나의 코어 단위의 CPU를 가지게 됨

  (*Traditionally, such systems have two or more processors, each with a single-core CPU.*)

- 프로세서들은 버스 구조와 클록, 메모리, 기타 주변기기 등을 공유함

- 멀티 프로세스의 제일 큰 장점은 싱글에 비해 __더욱 높은 처리량__ 을 보인다는 것으로, 더 많은 작업을 짧은 시간에 끝낼 수 있음

  - 하지만 N개의 프로세서가 있다고 해서, N개에 비례하는 퍼포먼스 증가량을 보이지는 않음
  - 프로세서가 증가하는 만큼, 전체 멀티 프로세서 구조를 유지하기 위한 작업의 부하가 올라가기 때문

  (*The speed-up ratio with N processors is not N, however; it is less than N. When multiple processors cooperate on a task, a certain amount of overhead is incurred in keeping all the parts working correctly. This overhead, plus contention for shared resources, lowers the expected gain from additional processors*)

  ![6](https://user-images.githubusercontent.com/68586291/135766000-18952296-f190-424f-8042-ef95b9633645.PNG)

- __`Symmetric Multiprocessing(SMP)`__ : 제일 흔한 형태의 멀티 프로세서 형태로, 프로세서들이 각각의 CPU를 가지고 있으며, CPU는 다시 각각의 레지스터와 캐시를 가지고 있음

  - 또한 시스템 버스 구조를 따라 프로세서들은 메인 메모리를 공유함
  - 이러한 모델은 여러 프로세스를 안정적으로 병렬로 처리 가능하게 함을 의미하지만 CPU가 각각 독립적인 프로세스에 위치하기 때문에 하나가 유휴상태이면서 다른 하나는 과부하된 상태가 발생할 수 있음
  - 프로세서들이 특정 자료 구조를 공유하도록 함으로써 프로세스와 자원들이 여러 프로세스들에 의해 공유됨으로써 특정 프로세서에의 과부하를 방지하도록 하는 것

![7](https://user-images.githubusercontent.com/68586291/135766450-cd40f493-2b0b-4104-8b09-fd61caeac6f7.PNG)

- 한편, __`멀티 코어(Multicore)`__ 개념으로도 멀티 프로세서 시스템을 확장시켜 이해할 수 있는데, 이는 하나의 칩(프로세서)에 여러 개의 코어가 존재하는 것을 의미함

  - 이러한 멀티 코어 시스템은 여러 프로세서를 가지고 있는 것보다 더욱 효율적인데, 여러 프로세서들 간의 커뮤니케이션 비용이 발생하지 않을 뿐더러 더 적은 에너지를 소비하기 때문

    (*Multicore systems can be more efficient than multiple chips with single cores because on-chip communication is faster than between-chip communication.*)

  - 위의 그림은 듀얼 코어 시스템 구조를 나타낸 것으로, 하나의 프로세서에 2개의 CPU가 존재하며 각각의 CPU가 다시 레지스터와 캐시(Level 1)를 가지고 있는 형태임

  - 캐시의 경우 Level 1 캐시가 독립적인 CPU가 가지고 있게 되며, Level 2 캐시를 CPU들이 공유하게 되며, 더 작은 규모의 Level 1 캐시가 더욱 빠른 처리 속도롤 보임

![8](https://user-images.githubusercontent.com/68586291/135766883-51d6b4c3-10d9-4de1-8dbf-6c601df1643f.PNG)

- 하지만 이러한 멀티 코어도, CPU를 늘리는 것만이 능사는 아님

  - 너무 많은 CPU를 보유하게 되면 시스템 버스에 가해지는 부하도 그만큼 커지기 때문에 나중에 성능 저하나 병목 현상을 유발할 수 있음

- 이럴 경우에는 각각의 CPU(혹은 일련의 CPU 집단)에 개별 로컬 메모리를 부여하여 작고 빠른 로컬 버스 구조를 통해 접근할 수 있는 방식을 적용할 수 있음

  (*An alternative approach is instead to provide each CPU with its own local memory that is accessed via a small, fast local bus.*)

- 또한 CPU들은 __`Shared system interconnect`__ 에 의해 서로 연결되어 하나의 물리적인 공간을 공유하게 되는데, 이를 __`Non-Uniform Memory access(NUMA)`__ 라고 부름
  - 위의 그림을 보면 각각의 CPU들이 각자의 메모리를 가지고 있으면서 서로 연결되어 있는 것을 확인할 수 있음 
  - 로컬 메모리 접근에 따른 속도 향상 뿐만 아니라 전체 시스템 버스와 달리 이러한 CPU 간의 국지적인 연결에는 큰 부하가 없기 때문에 프로세서의 확장에 있어 더욱 유연하다 할 수 있음
  - 하지만 이러한 NUMA의 장점에도 불구하고 CPU가 늘어날 때마다 CPU 간의 상호 연결을 따라 메모리에 접근(Remote Memory Acess)할 경우 발생하는 지연 시간이 그만큼 늘어난다는 잠재적인 결함이 있음
  - 따라서 운영체제는 이를 방지하기 위해 CPU Scheduling과 Memory Management에 더욱 신중을 기해야 할 것
- 마지막으로 __`Blade Servers`__  라 불리는, 멀티 프로세서 보드와 입출력 보드, 네트워킹 보드 등을 한 데 모아놓은 시스템 구조가 있음
  - 이것이 멀티프로세서 구조와 다른 점은, 각각의 블레이드 프로세서가 독립적으로 돌아가면서 고유의 운영체제를 가지고 있다는 점임
  - 하지만 본질적으로 이러한 서버 역시 여러 개의 독립적인 멀티 프로세서 시스템을 합쳐놓은 것으로 이해함으로써 어떤 의미에서는 멀티 프로세서가 좀 더 확장된 개념으로도 볼 수 있음

<br>

### 3-3. Clustered System

![9](https://user-images.githubusercontent.com/68586291/135874938-5e301d68-d01a-454c-b564-aff67bb7c9ce.PNG)

- 여러개의 CPU가 모여있는 멀티프로세서 시스템이지만, 2개 이상의 독립적인 시스템(혹은 노드)가 모여있는 구조라는 차이가 있음

  - 개별 시스템이 멀티코어 시스템이 되는 것

- Clustered Computer 들은 LAN(Local Area Network)을 통해 서로 연결되어 있으며 저장소를 공유하고 있음

- 이런 식의 군집화는 결국 더욱 높은 수준의 서비스를 제공하기 위한 것인데, 군집 내의 한두개의 시스템에 오류가 발생하더라도 나머지 시스템이 제대로 돌아가면 서비스가 제대로 제공될 수 있다는 것을 의미함

- __`Asymmetric Clustering`__ : 다른 기기가 어플리케이션을 실행시킬 동안, 어느 한 기기는 __`hot-standby mode`__ 상태로 있는 비대칭 클러스터링 방식을 의미

  - hot-standby mode 상태의 기기는 active 상태의 서버를 모니터링하는 역할을 수행함
  - 만일 실행중인 서버에 오류가 발생하게 되면, 모니터링 하던 서버가 active 상태로 전환되는 것

- __`Symmetric Clustering`__ : 비대칭적 방식과 달리, 2개 이상의 호스트가 어플리케이션을 실행시키면서 서로를 모니터링하는 방식임

  - 모든 사용가능한 하드웨어를 사용한다는 점에서 더욱 효율적이지만 동시에 적어도 하나 이상의 어플리케이션이 실행 가능해야 한다는 조건을 충족해야 함

- 한편, 클러스터링은 모든 컴퓨터가 어플리케이션을 동시에 실행할 수 있도록 병렬처리를 가능하게 하기 때문에 높은 성능의 컴퓨팅을 가능하게 함

  (*... they can run an application concurrently on all computers in the cluster.*)

  - 이 경우 병렬 처리되는 어플리케이션은 군집화의 장점을 살릴 수 있도록 설계되야 하는데, 이를 위해 사용되는 기술중 하나가 평행화(__`Parallelization`__) 임.
  - 이는 프로그램을 여러 요소로 나눈 후, 각 요소가 군집에 속하는 개별 코어들에 의해 병렬적으로 처리되도록 하는 방식을 말함
  - 일반적인 병렬 프로그래밍을 떠올리면 되듯, 개별 코어들에 의해 수행된 결과가 최종적으로 모여 최종 솔루션이 되는 것

- d여기서 더 나아가 여러 군집을 다시 군집화(*forms of clusters include paralell clusters*) 클러스터링을 LAN이 아닌 WAN(Wide Area Network) 범위에서 이루어지도록 하는 방식도 생각해볼 수 있음

  - 평행적으로 존재하게 될 군집들(Paralell Clusters)은 여러 호스트로 하여금 공유되는 저장소나 데이터 영역에 접근할 수 있도록 함
  - 하지만 대부분의 운영체제들은 여러 호스트들에 의한 동시적인 데이터 접근을 100% 처리하기에는 성능상의 한계가 있기 때문에, 이러한 방식은 보통 특수한 소프트웨어를 부가적으로 요구하는 경우가 많음
  - 대표적인 것이 __`Oracle Real Application Cluster`__ 

<br>

## 4. Operating-System Operations



---

## Reference

- [[컴퓨터구조]인터럽트(Interupt)란?](https://whatisthenext.tistory.com/147)

