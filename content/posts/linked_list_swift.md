

+++
title = "스위프트로 연결리스트(Linked List) 구현하기"
tags = ["자료구조","swift"]
categories = ["Computer Science"]
date = "2022-01-14"

+++

​        

## 1. 연결리스트(Linked List)

![이중 연결 리스트의 구조](https://upload.wikimedia.org/wikipedia/commons/c/ca/Doubly_linked_list.png)

- 추상형 자료형 중 __`리스트(List)`__ 를 구체화 한 자료구조로, 데이터를 포함하는 __`노드(Node)`__ 들이 다음(혹은 이전) 노드와 연결된 포인터를 통해 서로 연결되어 있는 형태이다.
- 일반적으로 __`단순 연결 리스트(Singly Linekd List)`__ 와 __`이중 연결 리스트(Doubly Linked List)`__ 로 나뉘며, 여기서는 이중 연결 리스트 형태로 구현하였다.
  - 이중 연결 리스트는 단순 연결 리스트와 달리 다음 노드와 함께 이전 노드를 가리키는 포인터도 존재한다.
  - 이중 연결 리스트로 구현할 경우에는 노드 삭제 시에 삭제할 노드의 이전과 다음 노드를 서로 연결할 때 이전 노드에 접근하기 더욱 편리해진다.
  - 일반적으로 연결 리스트에서 노드의 탐색 시 시간 복잡도가 O(N)이 되는데, 이전 노드를 참조하는 포인터가 존재하면 이를 찾는 데 O(1)로 줄어들기 때문이다.

​    

## 2. 스위프트로 연결리스트 구현하기

### 2-1. 노드(Node) 구현

- 위에서 언급한 대로 __`노드(Node)`__ 는 데이터를 포함하고 있으며, 포인터를 통해 이전 노드와 다음 노드의 위치를 알 수 있다.

- 리스트를 구성하는 노드(Node)가 될 클래스를 정의한다.
  - 노드는 담고 있는 데이터를 프로퍼티로 가지고 있다.
  - 노드에는 다양한 타입의 데이터가 들어가며, 이를 제네릭으로 __`<T>`__ 로 선언한다.
  - 노드 클래스의 프로퍼티는 데이터 뿐만 아니라 연결된 이전 노드(__`prev`__)와 다음 노드(__`next`__)에 대한 참조도 프로퍼티로 가지게 된다.

```swift
class Node<T>{

  var prev: Node<T>?
  var next: Node<T>?
  var data: T?

  init(prev: Node<T>?, next: Node<T>?, data: T?){
    self.prev = prev
    self.next = next
    self.data = data
  }
}
```

​    

### 2-2. Linked List

- 연결리스트 클래스 역시 제네릭 __`<T>`__ 를 선언하며, 여기서는 노드의 검색을 위해 해당 데이터가 비교 가능해야 하기 때문에 __`Equatable`__ 을 같이 선언한다.
- 연결 리스트 클래스는 리스트의 맨 처음과 끝에 해당하는 헤드 노드(__`head`__) 와 테일 노드(__`tail`__) 을 각각 프로퍼티로 가지게 된다.
- 연결 리스트 클래스는 각각 아래의 동작들에 대한 함수를 가지게 된다.
  - 리스트가 비어있는 지 여부 판별
  - 노드의 추가
  - 노드의 탐색
  - 노드의 삭제

- 위를 토대로 연결 리스트 클래스의 구조를 나타내면 아래와 같다.

```swift
class LinkedList<T: Equatable>{

  //헤드노드
  var head:Node<T>?
  //테일노드
  var tail:Node<T>?

  //생성자
  init(head: Node<T>?){
    self.head = head
    self.tail = head
  }

  //리스트 비어있는 지 판별
  func isEmpty()->Bool

  //맨 끝에 노드 추가
  func append(node: Node<T>)

  //입력받은 인덱스값에 해당하는 노드 검색
  func search(index: Int)->Node<T>?

  //입력받은 데이터를 가진 노드 검색
  func search(data: T)->Node<T>

  //리스트 사이즈 도출
  func size()->Int
  
  //원하는 인덱스에 노드 추가
  func insert(node: Node<T>, index: Int)

  //파라미터로 받는 데이터를 포함하는 노드 삭제
  func remove(data: T)
}
```

- 최종적으로 완성된 LinkedList 클래스는 아래와 같다.

```swift
class LinkedList<T: Equatable>{

  //헤드노드
  var head:Node<T>?
  //테일노드
  var tail:Node<T>?

  //생성자
  init(head: Node<T>?){
    self.head = head
    self.tail = head
  }

  //리스트 비어있는 지 판별
  func isEmpty()->Bool{
    return head==nil
  }

  //맨 끝에 노드 추가
  func append(node: Node<T>){
    if(isEmpty()){
      head = node
      tail = node
    }else{
      tail!.next = node
      node.prev = tail
      tail = node
    }
  }

  //입력받은 인덱스값에 해당하는 노드 검색
  func search(index: Int)->Node<T>?{
    var curr:Node<T> = head!
    if(index != -1){
      for _ in 0..<index{
        if curr.next == nil{
          return nil
        }else{
          curr = curr.next!
        }
      }
    }else{
      return nil
    }

    return curr
  }

  //입력받은 데이터를 가진 노드 검색
  func search(data: T)->Node<T>{
    var curr: Node<T> = self.head!
    while curr.next != nil{
      if(curr.data == data){
        return curr
      }
      curr = curr.next!
    }
    return curr
  }

  //리스트 사이즈 도출
  func size()->Int{
    if(isEmpty()){
      return 0
    }else{
      var size:Int = 1
      var curr: Node<T> = self.head!
      while curr.next != nil{
        size += 1
        curr = curr.next!
      }
      return size
    }
  }

  //원하는 인덱스에 노드 추가
  func insert(node: Node<T>, index: Int){
    if(head == nil){
      head = node
      tail = node
      return
    }

    //index = -1 인 경우에는 노드를 헤드노드 위치에 넣는 것으로 간주
    guard let prevTemp = search(index: index-1) else{
      //새로 넣을 노드와 헤드노드의 위치를 바꿔줌
      node.next = head
      head = node
      return
    }

    //nextTemp가 존재하지 않는 경우는 노드를 테일노드 위치에 넣는 것으로 간주
    guard let nextTemp = prevTemp.next else{
      //새로 넣을 노드와 테일노드의 위치를 바꿔줌
      prevTemp.next = node
      tail = node
      return
    }

    node.next = nextTemp
    prevTemp.next = node
  }

  //파라미터로 받는 데이터를 포함하는 노드 삭제
  func remove(data: T){

    let nodeToBeRemoved: Node<T> = search(data: data)
    if(nodeToBeRemoved.data == head!.data){
      self.head = head!.next
      head!.prev = nil
      return
    }else if(nodeToBeRemoved.data == tail!.data){
      self.tail = tail!.prev
      tail!.next = nil
      return
    }

    let prev: Node<T> = nodeToBeRemoved.prev!
    let next: Node<T> = nodeToBeRemoved.next!
    prev.next = next
    next.prev = prev

    nodeToBeRemoved.prev = nil
    nodeToBeRemoved.next = nil
  }
}
```

​        

## 3. 시간복잡도 검증

​        

## Reference

- [위키피디아 연결리스트](https://ko.wikipedia.org/wiki/%EC%97%B0%EA%B2%B0_%EB%A6%AC%EC%8A%A4%ED%8A%B8)
- [초보몽키의 개발공부로그](https://wayhome25.github.io/cs/2017/04/17/cs-19/)
