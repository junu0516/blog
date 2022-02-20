

+++

title = "옵셔널(Optional)과 옵셔널 체이닝(Optional Chaining)"
tags = ["swift"]
categories = ["Swift&iOS"]
date = "2022-01-24"

+++

​    

스위프트의 주요 특징 중 하나인 옵셔널과 옵셔널 체이닝에 대해 정리해보았다.

<img src="https://assets.alexandria.raywenderlich.com/books/sa/images/29222ecc50ebce9ea64b63614c7c08a055137e27842d7ce26afd1b3526413dbd/original.png" alt="image" style="zoom:40%"/>    

## 1. 옵셔널(Optional)

### 1-1. 옵셔널 선언하기

스위프트의 주요 특징 중 하나로 __`안전성(Safety)`__ 를 꼽을 수 있다. 스위프트는 엄격한 문법을 통해 개발자가 일으킬 수 있는 각종 실수를 방지하는데, 대표적인 것이 __`옵셔널(Optional)`__ 이다. 이전에 자바로 개발을 하다보면 런타임 도중 __`NullPointerException`__ 이 발생하는 경우가 많았다. 자바의 경우 변수에 null이 할당되어도 컴파일하는 데는 문제가 없었기 때문에 런타임 시점에 예상치 못하게 해당 예외가 종종 발생하곤 했다. 스위프트에서는 이를 방지하고자 일반적인 변수에  __`nil`__ 을 값으로 할당하면 컴파일 시점에 오류가 발생한다. 이런 상황에서 __값이 있을 수도 있고, 없을 수도 있는__ 변수의 선언에 사용되는 것이 옵셔널이다.

```swift
var str1: String = "" //값이 ""이지만, nil은 아님
var str2: String = nil //컴파일 오류
```

위와 같은 코드에서 str2과 같이 변수를 선언하면 컴파일 오류가 발생한다. 만일 여기에 값이 있을 수도 있고, 없을 수도 있는 경우라면 옵셔널을 선언해야 한다.

옵셔널의 선언은 __`Optional<T>`__ 혹은 타입 끝에 __`?`__ 을 붙이는 식으로 타입을 선언하며, 위의 경우에는 아래와 같이 옵셔널을 선언하면 된다. 

```swift
var str2: Optional<String>
var str3: String?
```

​    

### 1-2. 옵셔널 추출하기(Optional Unwrapping)

옵셔널에 있는 값을 추출하는 방식은 크게 __강제로 추출하는 것__ 과 __`옵셔널 바인딩`__ 을 통해 추출하는 것이 있다.

옵셔널의 __`강제 추출(Forced Unwrapping)`__ 은 제일 간단한 방식으로 옵셔널 값 끝에 __`!`__ 를 붙여주면 된다.

```swift
var str3: String?
var unwrapped: String = str3! 
```

위의 코드에서 unwrapped에 str3의 옵셔널에 있는 문자열 값을 강제로 추출하였다. 하지만 str3에 아무런 값을 할당하지 않았기 때문에 추출된 값은 __`nil`__ 이다. 따라서 위의 코드는 컴파일은 정상적으로 될 지 몰라도, 런타임 시점에 오류가 발생하게 되는 코드이다. 오류가 발생하지 않으려면 아래와 같이 nil이 아닌 값을 할당한 후 강제로 추출해야 한다.

```swift
var str3: String?
str3 = "String"
var unwrapped = str3!
print(str3) //String
```

즉, 옵셔널의 강제 추출은 위와 같이 __nil인 값을 강제로 추출할 경우 런타임 시점에 오류가 발생할 수 있는__ 위험이 존재하기 때문에 주의해야 한다.  

​    

__`옵셔널 바인딩(Optional Binding)`__ 은 강제 추출보다 안전하게 값을 추출하는 방식이다. __조건문을 통해 추출할 값이 nil인 지를 먼저 확인 하고, 아닐 경우에만 값을 추출하는 것__ 이라고 보면 된다. 아래의 코드를 통해 살펴보자.

```swift
var str4: String? = "Some String"
if let unwrapped = str4{
  print(unwrapped)
}else{
  print("nil")
}
```

임시로 상수(__`let`__) unwrapped를 선언한 후, 조건문을 통해 nil이 아닌 경우에는 문자열 값을 출력하는 코드이다. 만일 unwrapped가 상수가 아닌 변수(__`var`__) 인 경우에는 블록 안에서 값을 변경할 수 있다.

여러 개의 옵셔널 값을 추출하는 경우에는 아래와 같이  __`,`__ 를 통해 옵셔널 변수들을 열거하고 블록 안에서 여러 개의 값을 한 번에 처리할 수 있다.

```swift
var nameOpt: String? = "Jed"
var ageOpt: Int? = 28

if var name = nameOpt, var age = ageOpt{
  age += 1
  print("name: \(name), age: \(age)")
} 
```

​    

## 2. 옵셔널 체이닝과 빠른 종료

### 2-1. 옵셔널 체이닝(Optional Chaining)

__`옵셔널 체이닝(Optional Chaining)`__ 은 __여러 옵셔널이 마치 자전거 체인처럼 서로 꼬리에 꼬리를 무는 형태로 반복 사용된 형태를 말한다.__ 만일 연결된 일련의 옵셔널 중 __하나라도 값이 존재하지 않는다면 최종적으로 nil을 반환하게 된다.__

아래와 같은 클래스 구조가 있다고 가정해보자.

```swift
class Room{
  var number: Int
  init(_ number: Int){
    self.number = number
  }
}

//Building은 내부에 Room 타입 프로퍼티를 가지고 있음
class Building{
  var name: String
  var room: Room?

  init(_ name: String){
    self.name = name
  }
}

//Address는 내부에 Building 타입 프로퍼티를 가지고 있음
class Address{
  var city: String
  var building: Building?

  init(_ city: String){
    self.city = city
  }
}

//Person은 내부에 Address 타입 프로퍼티를 가지고 있음
class Person{
  var name: String
  var address: Address?

  init(_ name: String){
    self.name = name
  }
}
```

​    

Room->Building -> Address -> Person 순서로 각각의 타입을 내부에 프로퍼티로 두고 있는 형태이다. 여기서 Person 타입의 인스턴스를 하나 생성하고, Room 타입 인스턴의 number 값을 알기 위해 아래와 같이 옵셔널 체이닝과 강제 추출(Forced Unwrapping)을 적용해보았다.

```swift
//Person 인스턴스 선언
let person: Person = Person("jed")

//옵셔널 강제추출(여기서는 최종적으로 nil이 됨)
let roomNumberUnwrapped: Int = person.address?.building?.room?.number
print(String(roomNumberUnwrapped!)
```

위와 같은 방식으로 선언했을 때 roomNumberUnwrapped의 값은 nil이 된다. Person의 프로퍼티 중 address에 값이 없기 때문에 __옵셔널 체이닝 중간에 nil이 리턴된 것이다.__ 따라서 값을 강제로 추출하려고 하면 런타임 시접에 nil을 출력하려고 하기 때문에 오류가 발생한다.

​    

그렇다면 오류를 방지하기 위해 __`옵셔널 바인딩(Optional Binding)`__ 을 적용해보도록 하자.

```swift
//Person 인스턴스 선언
let person: Person = Person("jed")
var roomNumber: Int? nil

//옵셔널 바인딩을 적용해서 roomNumber 값 가져오기
if let address: Address = person.address{
  if let building: Building = address.building{
    if let room: Room = building.room{
      roomNumber = room.number
    }
  }
}

if let number: Int = roomNumber{
    print(number)
}else{
    print("Cannot find room number")
}
```

Address->Building->Room 타입 순서로 각각의 값이 존재하는 지를 차례대로 확인한 후, 최종적으로 roomNumber이 nil인 지 확인하기 때문에 런타임 시점에 오류가 발생하지 않을 것이다. 하지만 위의 코드는 __옵셔널 바인딩을 중첩사용하는 부분에서 들여쓰기가 많이 지기 때문에 가독성 측면에서 좋지 않다.__ 

​    

바로 여기서 옵셔널 바인딩 부분에 옵셔널 체이닝을 같이 적용하면 코드가 훨씬 간결해진다. __`if let {}`__ 형식의 nil 체크를 일일히 할 필요 없이 옵셔널 체이닝 한 번으로 여러 번의 옵셔널 바인딩을 한 번에 끝낼 수 있다. 

```swift
//Person 인스턴스 선언
let person: Person = Person("jed")

//옵셔널 바인딩에 옵셔널 체이닝 적용
if let roomNumber: Int = person.address?.building?.room?.number{
    print(roomNumber)
}else{
    print("Cannot find room number")
}
```

​    

값을 받아오는 것 외에도 __옵셔널 체이닝을 통해 값을 할당하는 것도 가능하다.__ 단, __값을 읽을 때와 마찬가지로 옵셔널 체이닝 중간에 nil이 없어야 한다.__

```swift
person.address?.building?.room?.number = 1000
print(person.address?.building?.room?.number) //nil
```

위의 경우에는 number까지 가기 전에 이미 address에 값이 없기 때문에  결과적으로 room 프로퍼티도 존재하지 않으므로 값을 할당할 수가 없다. 따라서 값을 할당하고자 한다면 __옵셔널 체이닝상에 있는 모든 프로퍼티에 값을 할당해야 한다.__

```swift
person.address = Address("seoul")
person.address?.building = Building("Geumho")
person.address?.building?.room = Room(500)
person.address?.building?.room?.number = 1000

print(person.address?.building?.room?.number) // Optional(1000)
```

​    

### 2-2. 빠른 종료(Early Exit)

일반적인 옵셔널 바인딩은 __`if-else`__ 구문을 통해 특정 조건을 만족하는 경우와, 그렇지 않은 경우에 대한 처리를 각각 해준다. 하지만 조건에 부합하지 않을 경우 해당 코드 블록을 좀 더 빠르게 종료하고자 한다면  __`guard`__ 키워드를 사용하는 것이 더욱 유리하다. __`guard`__ 구문은 __`guard-else`__ 형식을 사용하게 되며, 반드시 조건에 부합하지 않는 경우를 명시한 else 구문이 뒤따라와야 한다.

아래의 형식으로 __`guard`__  를 사용하면 되며, 만일 Bool타입 값이 false인 경우에는 해당 코드 블록을 종료하는 실행문과 같은 코드를 작성함으로써 __조건에 부합하지 않을 때 바로 실행을 종료하도록 처리할 수 있다.__

```
guard (Bool 타입 값) else{
    //조건에 부합하지 않을 경우 실행문
}
```

​    

```swift
// if-else
for i in 0...3{
  if i == 2 {
    print(i)
  }else{
    continue
  }
}

// guard
for i in 0...3{
  guard i == 2 else{
    continue
  }
  print(i)
}
```

위의 코드는 실제로 같은 기능을 수행하는 코드에서  __`if-else`__ 와 __`guard`__ 를 사용했을 때의 차이를 보여주는 예시이다. 후자의 경우 i가 2가 아닐 경우에는 __다른 구문을 실행하지 않고 그즉시 해당 분기를 즉시 건너 뛰도록(continue) 처리할 수 있으며, 굳이 조건을 만족하는 경우와 그렇지 않은 경우를 모두 일일히 명시할 필요가 없어 가독성 측면에서 더욱 유리하다.__

​    

그렇다면 2-1에서 사용했던 __`if-else`__ 형식으로 선언된 옵셔널 바인딩 + 옵셔널 체이닝을 __`guard`__ 구문으로 바꿔보도록 하자.

```swift
//Person 인스턴스 선언
let person: Person = Person("jed")

//옵셔널바인딩 + 옵셔널체이닝에 guard 적용
guard let roomNumber: Int = person.address?.building?.room?.number else{
  print("Cannot find room number")
  //값이 존재하지 않을 경우 바로 실행 종료
  return
}
print(roomNumber)
```

옵셔널 체이닝상의 모든 옵셔널을 차례대로 언래핑(__`unwrapping`__) 하면서 하나라도 값이 없을 경우에는 바로 __`else`__ 뒤의 예외 처리 구문을 실행한 후 해당 블록을 실행 종료한다.

단, __`if-esle`__ 와 다른 점은 __`guard`__ 구문은 __`return`__ , __`break`__ , __`continue`__ , __`throw`__ 등의 __제어문 전환 명령어를 사용할 수 있는 상황에서만 사용해야 한다.__ 즉, 함수와 같은 구문을 감싸고 있는 {} 코드블록이 반드시 존재해야 하는 것이다.

만일 바로 위의 예시 코드를 코드 블록으로 감싸져 있지 않은 채로 외부에 __`return`__ 없이 선언하게 되면 아래와 같은 컴파일 오류를 확인할 수 있을 것이다.

``` 
guard' body must not fall through, consider using a 'return' or 'throw' to exit the scope
```

<br>

## Reference

- 스위프트 프로그래밍: Swift 5(3판) - 야곰, 한빛미디어
- [Swift: 기초문법 [Unwrapping with guard] - 서근 개발노트](https://seons-dev.tistory.com/132)

​    
