

+++

title = "스위프트 클로저 개념 정리(작성중)"
tags = ["swift"]
categories = ["Swift&Ios"]
date = "2022-01-15"

+++

​    

자바를 사용하다가 스위프트를 사용하면서 체감하는 제일 큰 차이는 함수형 프로그래밍의 사용인 것 같다. 자바에서도 스트림으로 함수형 프로그래밍을 사용할 수 있지만 여전히 자바는 객체지향 언어로 쓰이는 것이 정석이다 보니 함수형 프로그래밍에 대해 이전까지는 제대로 알지 못했다. 스위프트를 사용하다보면 함수형으로 작성되어 {} 코드 블록을 인자로 넘기는 경우를 심심치 않게 보게 되는데 이를 클로저라고 부른다.



스위프트에서 함수형 프로그래밍 패러다임을 이해하기 위해  클로저(Closure)를 이해하는 것은 첫걸음이자 필수이다. 따라서 이번에는 클로저의 개념과 사용에 대해 정리해보았다.

​    

<img src="https://camo.githubusercontent.com/073d4db3e63450a4287978fd866b98ab9816dd2040cb87ef5395ec5a2f9a732e/68747470733a2f2f7377696674756e626f7865642e636f6d2f696d616765732f636c6f737572652d6573636170652e706e67" alt="image" style="zoom:33%"/>    

​    

## 1. 클로저 개념

스위프트에서 클로저는 {} 코드블록이 객체처럼 전달되면서 사용되는 독립적인 블록이다. 쉽게 말해 __이름이 없는 함수__ 의 형태라고 생각하면 된다. 따라서 함수는 __`func`__ 키워드가 붙은 클로저의 한 형태라 할 수 있다.

​    

클로저는 아래와 같이 세 가지 형태로 나눠볼 수 있다.

- 이름이 있지만, 값을 획득하지 않는 형태(전역 함수)

```swift
func print() {
  print("Hello")
} 	
```

- 내부 함수와 같이 이름이 있으면서 다른 함수 내부의 값을 획득할 수 있는 형태(중첩 합수)

```swift
func outer(){
  func print(_ name: String) {
    print("Hello \(name)")
  }
  var name: String = "Jed"
  print(name)
}
```

- 이름이 없고 주변 문맥에 따라 값을 획득하는 형태(축약 형태)

```swift
let num: Int = {return 0}
```

​    

위의 세 가지 예시처럼 클로저는 함수처럼 쓰이거나, 객체처럼 다른 변수에 할당할 수 있다. 스위프트에서 **함수는 1급 객체로 할당 받기, 인자로 전달하기, 반환하기 등이 모두 가능하다.** 스위프트에서 함수는 이름 없는 클로저이기 때문에, 결과적으로 클로저 역시 1급 객체이며 상수나 변수에 할당되거나 저장해서 사용 가능하다.

​    

## 2. 클로저 사용하기

### 2-1. 기본적인 클로저 사용하기

우선, 클로저의 표현은 기본적으로 아래의 형식을 따른다.

```swift
{
  (매개변수) -> 리턴타입 in
  	실행코드 + 리턴
}
```

매개변수와 리턴타입, 코드가 존재한다는 점에서 함수와 비슷한 형식이며 __`in`__ 은 실행 코드와 매개변수+리턴타입 선언부의 분리를 위한 것이다.

​    

바로 위에서 클로저를 다른 상수에 할당했기 때문에 여기서는 클로저를 함수의 인자로 전달하는 예를 살펴보도록 한다.    

스위프트에서 배열은 기본적으로 __`sort()`__ 라는 정렬 함수를 제공한다. 해당 함수에 아무런 인자를 전달하지 않으면 배열은 오름차순으로 정렬되며, 다른 기준으로 정렬하고자 할 경우에는 원하는 기준을 함수의 인자로 전달하면 된다. 

바로 여기서 별도의 정렬 기준을 전달할 때 이를 클로저 형식으로 전달할 수 있다.

```swift
var arr: [Int] = [1,3,2,5,4]
```

위와 같은 정수 배열에서 __`arr.sort()`__ 를 실행하면 배열이 [1,2,3,4,5]로 자동 정렬될 것이다. 이를 별도의 정렬 기준을 전달해서 내림차순으로 바꾸고 싶다면 아래와 같이 작성하면 된다. 

```swift
//내림차순 정렬
arr.sort(by:{(_ a: Int,_ b: Int)->Bool in
	return a>b
})
```

애플의 [공식 문서](https://developer.apple.com/documentation/swift/array/2296801-sort)를 보면 __`sort()`__ 의 정의를 아래와 같이 설명하고 있다.

```swift
mutating func sort(by areInIncreasingOrder: (Element, Element) throws -> Bool) rethrows
```

여기서 파라미터의 타입으로 __`(Element, Element) throws -> Bool`__ 이 선언된 것을 볼 수 있는데, 이는 2개의 인자를 받아 Bool 타입을 리턴하는 클로저를 해당 함수가 인자(by)로 받음을 의미한다. 클로저가 함수의 인자로 전달되는 대표적인 예인 것이다.

​    

### 2-2. 후행 클로저 사용하기

스위프트에서는 __`후행 클로저(Trailing Closure)`__ 를 사용할 수 있다. 함수의 마지막 인자에 클로저가 위치할 때, 소괄호를 닫은 후 클로저를 작성해도 되는 것인데 클로저의 길이가 길거나 괄호 안에 모든 인자를 명시하기에 가독성이 떨어진다 싶을 때 유용하다.

위의 __`sort()`__ 의 인자로 클로저를 넘길 때, 후행 클로저를 사용하면 아래와 같이 코드를 작성할 수 있다.

```swift
//기본 클로저
arr.sort(by:{(_ a: Int,_ b: Int)->Bool in
	return a>b
})

//후행 클로저
arr.sort(){(_ a: Int,_ b: Int)->Bool in
	return a>b
}
```

또한, 하나의 클로저만 인자로 전달할 경우에는 함수의 소괄호를 생략해도 된다.

```swift
arr.sort{(_ a: Int,_ b: Int)->Bool in
	return a>b
}
```

​    

주의할 점은 후행 클로저로 사용 가능한 클로저는 __전달인자의 맨 마지막에 위치한 것들만 가능하다는 점이다.__ 따라서 여러 개의 클로저를 인자로 전달할 경우에는 맨 마지막 클로저만 후행 클로저 형식으로 사용할 수 있다.

아래와 같이 클로저를 인자로 받는 함수가 존재한다고 가정해보자.

```swift
func calculate(_ a: Int, _ b: Int, _ increment:(Int, Int)-> Int,  _ subtract: (Int, Int) -> Int) -> Int {
  let left = increment(a,b)
  let right = subtract(a,b)
  return left+right
}

//마지막 인자를 괄호밖으로 빼줬음
var result = calculate(1,2,{$0+$1}){$0-$1}
print(result)
//2
```

위의 코드에서 __`calculate()`__ 의 마지막 인자인 __`subtract()`__ 에 해당하는 클로저를 소괄호 밖으로 빼줬다. __`sort()`__ 예시와의 차이는 __`calculate()`__ 는 전달인자의 개수가 여러개이다. 따라서 맨 마지막 클로저만 후행 클로저로 사용하되, 나머지 인자들의 존재로 인해 소괄호는 생략하지 못한 것이다.

​    

클로저가 여러개인 경우에는 __`다중 후행 클로저(Multiple Trailing Closure)`__ 문법을 사용할 수 있다. [애플 공식 문서](https://docs.swift.org/swift-book/LanguageGuide/Closures.html) 에서는 다음과 같은 상황에 다중 후행 클로저를 사용할 수 있다고 설명하고 있다. 아래와 같은 함수가 존재한다고 가정해보자.

```swift
func loadPicture(from server: Server, completion: (Picture) -> Void, onFailure: () -> Void) {
  if let picture = download("photo.jpg", from: server) {
    completion(picture)
  } else {
    onFailure()
  }
}
```

__`loadPicture()`__ 은 서버로부터 jpg 파일을 다운받아(__`download()`__)서 성공할 경우에는 __`completion()`__ 을 호출하고, 그렇지 않을 경우에는 __`onFailure()`__ 을 호출하는 함수이다. 특정 로직의 성공, 실패에 따라 각기 다른 처리를 클로저를 통해 명시하는 것이다.

이를 다중 후행 클로저를 통해 호출하면 아래와 같이 __첫 번째 클로저의 이름은 생략한 채로 전달 인자를 명시하는 소괄호 밖에 여러 개의 중괄호를 열고 닫음으로써 각 클로저를 표현할 수 있다.__

```swift
loadPicture(from: someServer) { picture in
	someView.currentPicture = picture //completion()에 해당함
} onFailure: {
  print("Couldn't download the next picture.")
}    
```

​    

### 2-3. 클로저를 간단하게 표현하기

위에서 살펴봤듯이 함수의 인자로 클로저를 전달할 때는, __함수에서 정의한 대로 전달인자와 리턴타입을 지켜야 한다.__ 따라서 함수를 호출했을 때 컴파일 오류 없이 클로저가 잘 전달되었다는 것은 __클로저가 형식을 제대로 지켰음을 의미한다.__ 문맥에 따라 넘겨진 클로저의 인자 타입과 리턴 타입 등을 유추할 수 있는 것이다.

2-1에서 다룬 __`Array.sort()`__ 를 다시 살펴보면 아래와 같은 형식으로 클로저를 인자로 넘길 것을 정의하고 있다.

```swift
sort(by areInIncreasingOrder: (Element, Element) throws -> Bool)
```

 여기서 클로저의 인자타입인 (Element, Element) 와 리턴타입 ->Bool 모두 문맥에 따라 유추 가능한 부분이다. 그래서 아래와 같이 이를 모두 생략한 채로 함수를 호출하는 것이 가능하다.

```swift
//before
arr.sort(by:{(_ a: Int,_ b: Int)->Bool in
	return a>b
})

//after
arr.sort{(a,b) in
	return a>b
}
```

​    

위의 코드에서 간략화된 부분을 보면 클로저의 전달인자 타입과 리턴타입을 생략했지만 여전히 전달 인자명인 __`a`__ , __`b`__ 가 반복적으로 사용되고 있다. 스위프트는 이러한 부분 역시 __단축 인자의 사용을 통해 인자의 이름을 일일히 사용하지 않아도 되도록 하고 있다.__

```swift
//before
arr.sort{(a,b) in
	return a>b
}

//after
arr.sort{ $0>$1 }
```

첫번째 전달인자부터 시작해서 $0, $1의 순서로 전달인자의 이름을 달러기호(__`$`__)와 숫자의 조합으로 표현하여 대체한 것이다. 이렇게 단축 인자를 사용하게 되면 클로저 내에서 전달 인자 부분을 표시할 필요가 없어지므로 자연스레 __`in`__ 키워드 역시 사용할 필요가 없어진다.

또한 마지막으로 __`return $0>$1`__ 과 같이 단 한줄의 실행문만 남은 경우라면 __`return`__ 도 생략할 수 있다.(마지막 실행문의 결과값이 리턴값이 되는 것으로 유추 가능하기 때문이다.)

​    

살펴본 클로저 문법의 축약법은 아래와 같이 요약할 수 있다.

- 클로저의 전달인자 타입과 리턴 타입을 생략할 수 있다.
- 전달인자 이름은 인자들이 선언된 순서대로 $0,$1과 같이 단축 인자로 대체할 수 있다.
- 단축 인자를 사용할 경우 전달인자를 명시하지 않아도 되며 __`in`__ 키워드를 생략할 수 있다.
- 위의 세 가지를 적용했을 때 한 줄의 실행문만 남으면 __`return`__ 키워드 역시 생략 가능하다.

​    

## 3. 값 획득(Capturing Values)

스위프트에서 클로저는 주변 문맥으로부터 상수나 변수를 획득하여 이를 내부에서 참조하거나 수정할 수 있다. 이럴 경우 상수나 변수가 정의된 기존의 블록이 메모리상에 존재하지 않는다 해도 해당 상수 및 변수의 참조는 여전히 가능하다.

​    

이러한 주변 문맥에서 값을 획득하는 것이 적용된 대표적 예가 중첩함수(내부함수)의 사용이다. 아래 코드를 통해 살펴보자

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int{
  var runningTotal=0
  func incrementer() -> Int{
    runningTotal += amount
    return runningTotal
  }
  return incrementer
}
```

__`makeIncrementer()`__ 의 리턴타입은 __`()->Int`__ , 즉 전달인자 없이 정수타입을 리턴하는 클로저이다. 내부에 __`incrementer()`__ 이라는 함수가 선언되있고, 이는 외부에서 __`runningTotal`__ 와 __`amount`__ 값을 참조하여 수정된 __`runningTotal`__ 값을 리턴한다. 이후 내부에 선언된 __`incrementer()`__ 이 최종적으로 __`makeIncrementer()`__ 의 리턴값이 된다. 함수도 클로저의 한 형태라는 점에서, 내부의 클로저가 외부로부터 변수를 획득하여 내부에서 수정한 것이다.

만일, 아래와 같이 내부의 __`incrementer()`__ 함수를 외부로 뺄 경우에는 겉으로만 봤을 때 문법적으로 이상한 코드가 된다. 아무런 전달 인자도 없기 때문에 내부에서 참조하고자 하는 __`runningTotal`__ 과 __`amount`__ 의 값이 어디있는 지 모르기 때문이다.

```swift
func incrementer()-> Int{
  //블록 내에서 runningTotal, amount값을 찾을 수 없음
  runningTotal += amount
  return runningTotal
}
```

하지만 __`makeIncrementer()`__ 의 내부함수 형태와 같이 블록 주변에 __`runningTotal`__ 과 __`amount`__ 가 정의되있다면 참조를 획득할 수 있다. 다시 말해, __`makeIncrementer()`__ 의 호출이 끝나더라도, 내부에 있던 __`runningTotal`__ 과 __`amount`__ 의 참조는 사라지지 않았기 때문에 이후에 __`incrementer()`__ 이 호출되더라도 계속해서 사라지지 않은 기존 값을 사용할 수 있는 것이다. 

아래의 예를 통해 살펴보자. 

```swift
let incrementByTwo: (() -> Int) = makeIncrementer(forIncrement: 2)
let first: Int = incrementByTwo()
let second: Int = incrementByTwo()
let third: Int = incrementByTwo()
print(first)//2
print(second)//4
print(third)//6
```

__`incrementByTwo`__ 는 __`runningTotal`__ 값이 0인 상태에서  __`inrementer()`__ 를 참조하고 있다.(함수가 객체처럼 취급되어 변수에 할당된 것이다.) 이후 __`first`__ , __`second`__ , __`third`__ 에 __`incrementByTwo()`__ 를 순서대로 호출하여 리턴한 값을 할당하면 차례 대로 2,4,6이 할당된 것을 확인할 수 있다.

이는 __`incrementer()`__ 이 세 번 호출된 것인데 각 차례마다 앞에서 수정된  __`runningTotal`__ 값을 참조하고 있기 때문에 2->4->6의 순서대로 값이 변화한 것이다. __`makeIncrementer()`__ 의 호출은 끝났지만 각각 인자와 내부에 선언된 값이던 __`amount`__ 와 __`runningTotal`__ 의 참조는 살아있었기 때문에 __`incrementer()`__ 이 호출될 때마다 살아있던 참조값을 획득하여 다시 값을 수정한 것이다.  

​     

위에서 살펴본 값 획득 예를 통해 하나 더 알 수 있는 사실은, __스위프트에서 클로저는 참조 타입(Closures are reference types)__ 라는 것이다.

__`incrementByTwo`__ 라는 상수에 __`makeIncrementer()`__ 이라는 함수를 할당한 것은 결국 이것이 리턴하는 클로저인 __`incrementer()`__ 을 할당한 것인데, 여기서 클로저를 값으로써 할당한 것이 아닌 클로저의 참조를 할당한 것이다. 따라서 상수 __`first`__ , __`second`__ , __`third`__ 는 __`makeIncrementer()`__ 이 맨 처음 리턴했던 클로저를 동일하게 가리키고 있던 것이다. 이로 인해 __`incrementer()`__ 이 세 번 호출되면서 결과적으로 동일한 __`runningTotal`__ , __`amount`__ 이 참조하는 값을 가져와 쓸 수 있던 것임을 알 수 있다.

​     

## 4. 탈출 클로저(Escaping Closure)

함수의 전달인자로 클로저가 전달될 때, __함수 종료 후 클로저가 호출되는 경우를 가리켜 클로저가 함수를 탈출한다고 표현한다.__ 클로저는 기본적으로 클로저가 속한 함수 블록(스코프) 안에서만 사용할 수 있다.  따라서 함수의 호출이 종료된 후 클로저를 호출하는 것은 기본적으로 불가능하며, 클로저는 기본적으로 비탈출 클로저이다. 

하지만 상황에 따라서 외부 변수가 함수 내부의 클로저값을 참조해야할 수도 있고, 함수의 호출이 종료된 이후에 클로저를 호출해야하는 경우가 있을 수도 있다. 스위프트에서는 탈출 클로저를 선언하여 클로저가 함수 밖을 탈출하도록 할 수 있다.

​    

클로저를 인자로 전달할 때 변수명 옆의 __`:`__ 뒤에 __`@escaping`__ 어노테이션을 붙여 탈출 클로저임을 명시할 수 있으며, 이후 해당 클로저는 함수 외부에서 사용 가능하게 된다. 

클로저가 함수를 탈출한다는 것이 구체적으로 어떤 의미인지 살펴보도록 하자. 아래 코드는 클로저 타입의 외부 변수에 __`setClosure()`__ 을 통해 클로저를 할당하는 코드이다.

```swift
var someClosure: ()->Void = {}
func setClosure(_ closure: ()->Void){
  someClosure = closure
}
setClosure({print("hello")})
someClosure()
```

위의 코드를 실행하려고 하면 Xcode에서 아래와 같은 메시지가 나타나는 것을 확인할 수 있을 것이다.

```
Assigning non-escaping parameter 'closure' to an @escaping closure
```

전달인자로 넘긴 __`closure`__ 은 탈출 클로저 선언을 하지 않았기 때문에 비탈출 클로저이다. 그런데 함수 외부의 __`someClosure`__ 에 __`closure`__ 이 참조하는 클로저값을 넘기는 것은 곧, 해당 클로저가 함수 외부로 호출될 수 있음을 의미하기 때문에 위의 코드는 전달인자로 넘긴 클로저를 탈출 클로저로 선언해야 한다. 

```swift
//탈출 클로저 선언
func setClosure(_ closure: @escaping ()->Void){
  someClosure = closure
}
```

​    

## 5. 자동 클로저(Auto Closure)

자동 클로저는 함수의 전달 인자로 사용되는 코드를 자동으로 클로저로 만들어주는 것을 말한다. 자동 클로저의 사용은 탈출 클로저와 동일하게 함수 선언 시에 __`:`__ 옆에 __`@autoclosure`__ 를 선언을 통해 가능하다. 단, 자동 클로저로 사용하기 위한 클로저는 __인자가 없고 리턴값만 존재하는 형태여야 한다.__



위에서 살펴본 __`setClosure()`__ 에 자동 클로저를 적용해보도록 하자.

```swift
var someClosure: ()->Void = {}
func setClosure(_ closure: @escaping ()->Void){
  someClosure = closure
}
setClosure({print("hello")})
someClosure()
```

여기서 __`setClosure()`__ 을 호출하여 전달 인자로 {print("hello")} 라는 클로저를 넘겼다. __이는 전달인자는 없고 리턴값은 Void인 클로저이다.__ 여기서 __`closure`__ 를 아래와 같이 자동클로저를 적용하면 아래와 같이 __`setClosure()`__ 을 호출할 수 있다.

```swift
func setClosure(_ closure: @autoclosure @escaping ()->Void){
  someClosure = closure
}
setClosure(print("hello"))
```

전달인자로 클로저를 넘길 때 중괄호__`{}`__ 없이 일반적인 코드를 넘기면 이를 알아서 감싸서 클로저로 처리해주는 것이다. 단, __자동 클로저는 기본적으로 비탈출클로저__ 이기 때문에 위와 같이 클로저를 탈출시키려면 __`@escaping`__ 을 같이 선언해야 한다.

​    

## Reference

- 스위프트 프로그래밍: Swift 5(3판) - 야곰, 한빛미디어
- [Swift) 클로저(Closure) 정복하기(1/3) - 클로저, 누구냐 넌](https://babbab2.tistory.com/81)
- [애플 스위프트 공식문서](https://docs.swift.org/swift-book/LanguageGuide/Closures.html)
