

+++
title = "스위프트로 연결리스트(Linked List) 구현하기"
tags = ["자료구조","swift"]
categories = ["Computer Science"]
date = "2022-01-14"

+++

> 연결리스트에 대해 알아보고 스위프트로 연결리스트를 구현해보았다 ...

​    

## 1. 연결리스트

​    

## 2. 스위프트로 연결리스트 구현하기

### 2-1. Node

- 리스트를 구성하는 노드(Node)가 될 클래스를 정의한다.
  - 노드는 담고 있는 데이터를 프로퍼티로 가지고 있다.
  - 노드에는 다양한 타입의 데이터가 들어가며, 이를 제네릭으로 __`<T>`__ 로 선언한다.
  - 노드 클래스의 프로퍼티는 데이터 뿐만 아니라 연결된 이전 노드와 다음 노드에 대한 참조도 프로퍼티로 가지게 된다.

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

​       
