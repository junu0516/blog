

+++
title = "[자료구조] 자바로 스택과 큐 구현하기"
tags = ["자료구조","CS스터디"]
categories = ["자료구조"]
date = "2021-10-10"

+++



> 자바로 스택과 큐에 대해 알아보도록 하자

<img src="https://media.vlpt.us/images/nmy0502/post/e3ba79be-8d03-42c0-b1d6-22f70d0b5220/stack,%20queue.png" alt="Linear Structure(Stack, Queue)" style="zoom: 67%;" />

<br>

## 1. 스택(Stack)

```
스택의 개념과 활용, 자바를 통한 스택 구현 세 가지를 학습해보자!
```

### 1-1. 스택의 개념

- 데이터의 입출력이 __후입선출(LIFO, Last In First Out)__ 에 따라 이루어지는 자료구조

- 데이터의 스택에 추가하는 것을 __`push`__ , 스택으로부터 데이터를 꺼내는 것을 __`pop`__ 이라고 부름

- 후입선출이기 때문에 데이터가 들어오고 나가는 위치가 동일하며, 이러한 푸시와 팝이 이루어지는 위치를 __`top`__ 이라고 함

- 스택의 제일 흔한 활용은 자바 어플리케이션에서 __메소드를 호출하고 실행하는 상황__ 을 예로 들 수 있음

  ```java
  public void method1(){
    //...
  }
  
  public void method2(){
    //...
  }
  
  public void method3(){
    method1();
    method2();
  }
  
  //메인메소드 호출
  public static void main(String[] args){
    method3();
  }
  ```

  - 위의 상황에서 메소드가 호출되고 실행되는 순서는 아래와 같음
    - Main 메소드의 실행(푸시)
    - 3번 메소드 호출(푸시)
    - 1번 메소드 호출(푸시)
    - 1번 메소드 종료(팝)
    - 2번 메소드 호출(푸시)
    - 2번 메소드 종료(팝)
    - 메인 메소드 종료(팝)
  - 메소드의 호출 순서대로 푸시가 일어나고, 호출 순서 반대로 팝이 일어나는 것을 알 수 있음
  - 자바 외에도, 컴퓨터의 메모리 구조에서 메소드의 호출 관련 부분을 저장하는 영역은 이러한 스택 자료구조를 사용하고 있음

<br>

### 1-2. 스택의 활용

```
위에서 말한 것 외에도, 컴퓨터 곳곳에서 다양하게 스택을 응용하고 있다!
```

- 변수에 대한 메모리 할당과 수집을 위한 시스템 스택
  - 자바를 예로 들면, __`heap`__ 영역에 생성된 객체들이 저장되며, 이러한 __객체를 참조하는 변수가 스택에 저장됨__
  - 또는  int, char 과 같은 원시타입의 데이터 또한 스택에 저장됨(객체변수와 달리 참조값이 아닌 실제 데이터를 직접 저장하는 것)
  - 시스템 스택에는 변수의 선언과 동시에 순서대로 변수를 저장할 공간이 할당되며, 마지막에 저장된 변수부터 다시 스택에서 제거되는 것
- 서브루틴의 호출 관리를 위한 스택
  - 반복되는 작업을 별도로 묶어 이름을 붙인 것을 __`서브루틴`__ 이라고 하며, 위에서 언급한 메소드 혹은 함수 호출의 예시가 바로 여기에 해당됨
- 인터럽트 처리와, 이후에 리턴할 명령의 수행 지점을 저장하기 위한 스택
  - __`인터럽트(interrupt)`__ : 프로그램이 동작하는 도중에 발생하는 의도치 않은 사건들을 의미함
  - 운영체제는 외부 장치의 동작과 전체 시스템의 동작 간의 안정적인 조화를 위해 인터럽트를 사용
  - 프로램이 실행되고 있는 도중에 외부장치에 의한 인터럽트 신호가 들어올 경우 운영체제는 현재 상태를 __`프로세스 제어블록(PCB)`__ 에 저장한 후, 인터럽트 요청을 처리하고 다시 원래의 상태로 돌아오게 됨
  - 여기서 프로세스 제어 블록이 저장되는 곳이 스택 메모리 영역이며, 인터럽트 처리 후 다시 복귀해야 할 명령의 수행 지점에 대한 정보가 저장됨
- 컴파일러, 순환호출 관리

<br>

### 1-3. 스택 구현하기

```
여기서는 크게 스택의 생성, 삽입, 삭제, 탐색에 대한 처리를 구현해보도록 한다.
```

​    

- __배열을 활용한 경우__ : int 타입의 데이터만을 담는 스택을 배열을 활용하여 구현해보았음

```java
public class MyStack{
  private int stackSize; //스택 용량
  private int top; //top(push/pop이 일어나는 지점)
  private int[] stack; //스택 본체
  
  public StackExample(int size){
    this.top = -1;
    this.stackSize = size;
    stack = new int[size];
  }
  
  //예외처리 1. 스택이 비어 있는 경우
  public class EmptyStackException extends RuntimeException{
    public EmptyStackException(){}
  }
  
  //예외처리 2. 스택이 가득 찬 경우
  public class OverFlownStackException extends RuntimeException{
    public OverFlownStackException() {}
  }
  
  public int push(int data){
    
    if(top>=size){ //스택이 가득 찬 경우(배열의 크기 범위를 벗어난 것)
      throw new OverFlownStackException();
    }
    //데이터를 넣고, top값을 1 증가시킴
    return stack[top++] = data;
  }
  
  public int pop() throws EmptyStackException{
    
    if(top<0){ //스택이 비어있는 경우(top이 초기값 그대로인 것)
      throw new EmptyStackException();
    }
    
    //포인터 값을 1 감소시킨 후, 감소된 포인터 위치에 있는 데이터를 리턴
    return stack[--top];
  }
  
  //peek() 메소드는 현재 top에 있는 데이터 값이 무엇인지를 알려줌
  public int peek() throws EmptyStackException{
    
    if(pointer<=0){//스택이 비어있는 경우에는 알려줄 값이 없으므로 예외발생
      throw new EmptyStackException();
    }
    
    //top에 있는 데이터를 리턴
    return stack[top];
  }
  
  public boolean empty(){
    return top<0; //top=-1일 경우에는 스택이 빈 것이므로 true
  }
  
  //특정 데이터의 위치값을 찾음(없는 경우에는 -1 리턴)
  public search(int data){
    
    for(int i=0;i<=top;i++){
      if(stack[i] == data){
        return i;
      }
    }
    return -1;
  }
  
  /*
  	이 외에도, 스택이 비어있거나 가득 찼는 지 여부 혹은 스택에 존재하는 데이터의 수를 반환하는 메소드 등을 배열을 통해 정의해볼 수 있음
  	(간단하니 여기서는 생략하도록 한다 ...)
  */
}
```

<br>

- 연결리스트(LinkedList) 구조를 활용하여 스택을 구현할 수도 있음
  - 단순 연결리스트의 구조를 활용하며, 이는 각 노드들 간의 연결이 단방향적이라는 특징을 가짐
  - 마지막 노드의 포인터는 null을 가리키는 것

```java
//개별 노드 클래스의 생성
public class Node{
    private int data; //노드에 담을 데이터
    private Node nextNode; //해당 노드에 연결된 다음 노드
    
    public Node(){
       
    }
 
  public Node(int data){
    	this.data = data;
        this.nextNode = null;
    }
	
    //다음 노드와의 연결 동작
    protected void linkNode(Node node){
    	this.nextNode = node;
    }
    
    //노드에 담긴 데이터 반환
    protected int getData(){
    	return this.data; 
    }
    
    //연결된 다음 노드 반환
    protected Node getNextNode(){
    	return this.nextNode;
    }
}

// 단순 연결리스트 구조를 활용한 스택 클래스 선언
public class MyStack{
   private Node top; //가장 최근에 추가된 노드
   
   public MyStack(){
        this.top = null;
   }
    
    public void push(int data){
        Node newNode = new Node(data);
        newNode.linkNode(top);
        top = newNode;
    }
    
    public void pop(){
     	 top = top.getNextNode();
    }
    
    public int peek(){
    	if(empty()){
        	//예외처리
        }
    	return top.getData(); 
    }
    
    public boolean empty(){
    	return this.top = null;
    }
    
    //해당 데이터를 포함하는 노드를 검색
    public search(int data){
    	Node searchNode = top;
        int index = 1;
        while(true){
            if(searchNode.getData() == data){
            	d
                return index;                
            }
            else{
                searchNode = searchNode.getNextNode();
                index++;
                if(searchNode == null){
                    break;
                }
            }
        }
        return -1;
    }
}

```







---

## Reference

- 

