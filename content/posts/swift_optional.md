

+++

title = "옵셔널과 옵셔널 체이닝(작성중)"
tags = ["swift"]
categories = ["Swift&Ios"]
date = "2022-01-24"

+++

​    

스위프트의 주요 특징 중 하나인 옵셔널과 옵셔널 체이닝에 대해 정리해보았다.

<img src="https://assets.alexandria.raywenderlich.com/books/sa/images/29222ecc50ebce9ea64b63614c7c08a055137e27842d7ce26afd1b3526413dbd/original.png" alt="image" style="zoom:40%"/>    

## 1. 옵셔널(Optional)

### 1-1. 옵셔널 선언하기

스위프트의 주요 특징 중 하나로 __`안전성(Safety)`__ 를 꼽을 수 있다. 스위프트는 엄격한 문법을 통해 개발자가 일으킬 수 있는 각종 실수를 방지하는데, 대표적인 것이 __`옵셔널(Optional)`__ 이다. 이전에 자바로 개발을 하다보면 런타임 도중 __`NullPointerException`__ 이 발생하는 경우가 많았다. 자바의 경우 변수에 null이 할당되어도 컴파일하는 데는 문제가 없다. 하지만 스위프트에서 일반적인 변수에  __`nil`__ 을 값으로 할당하면 컴파일 시점에 오류가 발생한다. 이런 상황에서 __값이 있을 수도 있고, 없을 수도 있는__ 변수의 선언에 사용되는 것이 옵셔널이다.

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

## Reference

- 스위프트 프로그래밍: Swift 5(3판) - 야곰, 한빛미디어
