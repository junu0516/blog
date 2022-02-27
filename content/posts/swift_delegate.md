

+++

title = "스위프트의 Delegate Pattern 정리"
tags = ["swift"]
categories = ["Swift&iOS"]
date = "2022-02-19"

+++

<br>

스위프트로 다양한 ViewController을 다루면서 Delegate Procotol이라는 개념을 처음 접하게 되었다. 간단히 말해 특정 이벤트나 액션이 일어나면, 미리 위임자로 지정된 delegate object가 다른 객체를 대신해 특정 행위를 하게되는 형태를 의미한다. 

신기하면서도 매우 생소하게 다가오는 개념이라 한 번 다시 정리해보고자 한다..!

<img src="https://user-images.githubusercontent.com/68586291/154847216-f3efbd1c-92ab-4bab-99e7-1598baa6f222.png" alt="image" style="zoom:70%;"/>

## 1. UIImagePickerController 사용

>  우선 UIImagePickerController을 사용하면서 Delegate Protocol에 대해 알게된 과정을 정리해보고자 한다.  

### 1-1. UIImagePickerController

​    

<img src="https://user-images.githubusercontent.com/68586291/154623719-999c412d-e4fb-4a6e-8d8b-cc83277b00b3.gif" alt="image" style="zoom:50%;"/>

​    

__`UIImagePickerController`__ 은 위의 이미지와 같이 시스템 인터페이스를 다룸으로써 라이브러리에서 사진을 가져오거나, 직접 사진을 촬영하는 등의 상황에서 사용하는 ViewController(이하 뷰컨트롤러)이다.



 [공식문서](https://developer.apple.com/documentation/uikit/uiimagepickercontroller) 의 설명을 보면 해당 컨트롤러는 사진의 선택 혹은 촬영과 같은 사용자의 동작에 대한 결과를 __`delegate object`__ 에 전달하며, __`delegate object`__ 는 __`UIImagePickerControllerDelegate`__ 라는 프로토콜의 요구사항을 준수(conform)해야 함을 아래와 같이 명시하고 있다.

```
you must provide a delegate that conforms to the UIImagePickerControllerDelegate procotol.
```

실제로 아래 예시의 뷰컨트롤러 클래스가 상속받는  __`UIViewController`__ 의 내부 구조를 보면 프로퍼티로 __`delegate`__ 라는 프로퍼티를 가지고 있는 것을 확인할 수 있는데, 바로 이것이 위에서 언급하는 __`delegate object`__ 에 해당된다.

```swift

class MyViewController: UIViewController, UIImagePickerControllerDelegate {
		
  	private var imagePicker = UIImagePickerController()
  	@IBOutlet weak var selectButton: UIButton!
  
    @IBAction func selectButtonTouched(_ sender: UIButton) {
      //버튼이 눌리면 사진 선택 화면(View)를 보여줌
      self.present(imagePicker, animated: true, completion: nil)
    }
  
    override func viewDidLoad() {
        super.viewDidLoad()
        //UIImagePickerController의 delegate object로 현재 MyViewController을 지정
        imagePicker.delegate = self
        //사진앨범에서 이미지를 가져올 것임을 명시
        imagePicker.sourceType = .photoLibrary
    }
}
```

위의 코드는 __`UIImagePickerControllerDelegate`__ 프로토콜을 채택한 간단한 뷰컨트롤러 클래스 코드이다. 작성한  __*imagePicker.delegate = self*__  는 클래스가 가지고 있는 __`UIImagePickerController`__ 인스턴스의 __`delegate object`__ 가 자기 자신(MyViewController)이 될 것임을 의미한다. 또한 이를 위해 해당 클래스가  __`UIImageControllerDelegate`__ 을 채택하고 있음을 명시하였다.

이후 선택 버튼(selectButton)을 누르면 미리 지정해둔 __`selectedButtonTouched`__ 액션 메소드가 실행되면서 화면에 사진선택을 위한 창이 뜨게 된다.

<br>

### 1-2. UIImagePickerControllerDelegate

그렇다면 채택한 __`UIImageControllerDelegate`__ 프로토콜은 구체적으로 무슨 역할을 하는 걸까?

 [공식문서](https://developer.apple.com/documentation/uikit/uiimagepickercontrollerdelegate) 에서는 해당 프로토콜에 대해 아래와 같이 정의하고 있다.

```
A set of methods that your delegate object must implement to interact with the image picker interface.
```

간단히 말해 image picker 인터페이스와 관련된 __사용자 동작의 결과를 처리하는 메소드들을 담고 있는 프로토콜로 이해할 수 있다.__  동작의 결과를 처리하는 메소드들은 결과적으로 프로토콜을 채택한 __`delegate object`__ 내부에서 구현되야 할 것이다.

​    

```swift
class MyViewController: UIViewController, UIImagePickerControllerDelegate {
  /*
      나머지 부분 생략
  */
  
  //UIImagePickerControllerDelegate의 필요 메소드 구현
  func imagePickerController(_ picker: UIImagePickerController, 
                             didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]){
    
    //라이브러리로부터 가져온 이미지(UIImage)
    let selectedImage = info[UIImagePickerController.InfoKey.originalImage] as? UIImage
    
    //뷰컨트로러의 imageView의 image로 가져온 이미지를 할당
    self.imageView.image = selectedImage
    
    //화면에 띄웠던 UIImagePickerController의 View를 해제
    picker.dismiss(animated: true, completion: nil)
  }
}
```

위의 코드의 __`imagePickerController()`__ 메소드가 바로 앞서 언급한 사용자 동작의 결과를 처리하는 메소드가 된다.  공식문서에서는 해당 메소드에 대해 *'Tells the delegate that the user picked a still image or movie.'* 와 같이 __`delegate object`__ 에 사용자의 이미지 선택의 결과를 전달하는 것으로 설명하고 있다.

​    

위와 같이 작성한 후 실제로 __`selectButtonTouched`__ 메소드를 실행시키면 앨범에서 사진을 선택하는 창이 나타나면서  __`imagePickerController`__ 메소드의 내부 로직이 실행되는 것을 확인할 수 있다. 별 것 아닌 것처럼 보이지만, 실제로 뷰컨트롤러 내부에서 __`imagePickerController`__ __메소드를 직접적으로 호출하는 로직을 명시하지 않았음에도 불구하고 메소드 내부의 로직이 자동으로 실행된 것을 확인할 수 있다.__   

<br>

## 2. Delegate Pattern

### 2-1. Delegation의 정의

다음은 [위키피디아](https://en.wikipedia.org/wiki/Delegation_pattern)의 __Delegation Pattern__ 에 대한 설명 중 일부이다.

```
In delegation, an object handles a request by delegating to a second object (the delegate).
The delegate is a helper object, but with the original context. (중략)
```

해석하면 __`delegate object`__ 는 __위임자, 대리자__ 라는 사전적 정의에 맞게 특정 객체로부터 __외부 요청에 대한 처리를 위임받아 대신 수행하는 helper의 역할을 수행하는 second object가 된다.__ 앞서 살펴본  __`UIImagePickerController`__ 의 예를 다시 살펴보면 뷰컨트롤러 클래스는 __`UIImagePickerController`__ 의 위임자로서, 사진을 선택하는 사용자의 동작 결과를 대신 처리하는 역할을 수행한다.

​    

Swift에서는 이런 식의 위임 객체를 지정하여 처리를 대신 하도록 하는 __Delegation Pattern__ 이 흔히 적용된다. 이를 적용하기 위해서는 크게 아래의 3가지 요소가 필요하다.

- __위탁자 객체__ (delegator)
- __위임자 객체__ (delegate)
- __책임 위임__ (위임자에게 위탁자 정보 전달 / delegate protocol)

1의 예시에서 __`MyViewController`__ 이 위임자, __`UIImagePickerController`__ 가 위탁자, 책임의 위임은 __`UIImagePickerControllerDelegate`__ 에 각각 해당한다고 보면 된다.

<img src="https://user-images.githubusercontent.com/68586291/154850655-7b768528-916a-453c-bbe5-75cf5ae4033a.png" alt="image" style="zoom:80%;"/>

정리해보면 우선  __`UIImpagePickerController`__ 은 사용자의 동작 결과를 처리하는 일을 __`MyViewController`__ 에 위임(delegate) 하였고, 해당 뷰컨트롤러는 __`UIImagePickerControllerDelegate`__ 를 채택함으로써 __`delegate object`__ 로서 기능하게 된다. 

이후 UIImagePickerController은 사용자 동작의 결과를 __`UIImagePickerControllerDelegate`__ 를 거쳐 __`MyViewController`__ 에 전달하게 되며, __`MyViewController`__ 내부에서는 __`UIImagePickerControllerDelegate`__ 를 채택하면서 구현한  __`imagePickerController()`__ 메소드가 실행되는 것이다.

<br>

### 2-2. Delegate Pattern을 사용하는 이유

> 객체지향 프로그래밍에서 Delegate Object를 굳이 사용하는 이유에 대해 알아보자

개인적으로 __`UIImagePickerController`__ 을 사용하고 Delegate Pattern을 학습하면서 든 생각은 __*이걸 굳이 왜 사용하지?*__ 였다. 

```
Delegation is a way to make composition as powerful for reuse as inheritance [Lie86, JZ91]. 
```

결론부터 이야기하자면, Delegate Pattern을 통해 __객체 간의 합성을 통한 코드의 재사용__ 을 극대화할 수 있기 때문이다. 여기서 __객체의 합성(Object Composition)__ 은 __한 객체가 다른 객체의 참조 정보 등을 얻는 것__ 을 의미한다. 한 객체가 다른 객체의 기능을 사용해야 할 경우에 두 객체 간의 합성이 가능하다면, 중복되는 코드를 작성해도 되지 않기 때문에 재사용성이 올라가는 효과를 볼 수 있다.

<br>

<img src="https://miro.medium.com/max/749/0*J_Dm57bKTppN51oZ.png" alt="image" style="zoom:50%;"/>

<br>

보통 OOP에서 코드의 재사용성을 높이기 위한 또 다른 방법으로 두 객체 간의 __상속(Inheritance) 관계__ 를 정의하는 것을 떠올려볼 수 있다. 자식 클래스가 부모 클래스를 상속받아, 부모 클래스에서 정의된 기능을 그대로 사용하는 것이다. 위의 그림처럼 House 클래스가 Building 클래스를 상속받은 경우라면, House 객체를 생성한 후 건물의 역할과 관련된 처리는 부모 메소드에서 처리하고 후속으로 집에 대한 처리가 필요하다면 그 책임은 자식 메소드에서 처리하는 구조를 생각해볼 수 있을 것이다. 이처럼 Building을 상속받은 모든 객체는 Building의 기능을 (접근만 가능하면) 모두 가져다 사용하면 되기 때문에 코드의 재사용성이 결과적으로 높아지는 것이다.

하지만 두 객체의 특성상 명확한 계층관계가 존재하지 않는 상황이라면 아래와 같은 이유로 __객체 간의 상속은 지양된다.__

- 부모 클래스의 구현이 자식 클래스에 다 드러나기 때문에 본질적으로 __캡슐화(Encapsulation) 를 충족했다고 보기 어렵다.__
- 자식 클래스는 부모 클래스에 __종속적이기 때문에, 부모 클래스의 변경이 자식 클래스의 변경을 초래하는 경우가 많아진다.__

따라서  이러한 경우에는 코드의 재사용성을 위해 객체 간의 합성을 추구하는 것이 바람직하다. 두 객체가 __서로 간의 구현 정보를 알 수 없기 때문에, 각 객체는 각자의 역할에만 집중하면 되고 한 클래스의 변경이 다른 클래스의 변경을 초래할 가능성이 더 적기 때문이다.__

<br>

__객체 간의 위임(Delegation)__ 은 전반적으로 __객체 간의 합성을 실현하면서 객체 간의 상속의 이점을 누리도록 하는 패턴이다.__ 위탁자 객체가 특정 연산 처리에 대한 책임을 위임자 객체에 넘기는 것은 상속 관계에서 자식 객체가 부모 객의 메소드를 호출하여 연산을 처리하는 것과 유사하다. 하지만 앞서 살펴본 __`MyViewController`__ 과 __`UIImagePickerController`__ 의 경우처럼 __종속 관계 없이 특정 이벤트의 처리 요구 혹은 처리 결과를 전달__ 해주는 프로토콜을 통해 두 객체가 __상대적으로 가볍게 연결된 형태이기 때문에 결합도가 그만큼 낮다는 이점이 있다.__

__`MyViewController`__ 과 __`UIImagePickerController`__ 객체는 캡슐화로 인해 서로 간의 구현 정보를 모르지만, __`UIImagePickerControllerDelegate`__ 에 의해 서로 연결된 상태에서 이벤트 처리 요청 및 처리 결과를 서로 간에 전달함으로써 각각 이미지 선택 및 편집 및 선택된 이미지 처리라는 책임에 집중할 수 있는 것이다.

​        

따라서 이러한 이유로 __`UIImagePickerControllerDelegate`__ 를 비롯하여 UITableViewDelegate, UITextFieldDelegate 등 iOS 개발에서 많은 UI 요소에 Delegate Pattern이 사용된다고 볼 수 있다.

<br>

## Reference

- [UIImagePickerControllerDelegate - 애플 공식문서](https://developer.apple.com/documentation/uikit/uiimagepickercontrollerdelegate)
- [위키피디아 Delegation Pattern](https://en.wikipedia.org/wiki/Delegation_pattern)
- [위키피아 Delegation](https://en.wikipedia.org/wiki/Delegation_(object-oriented_programming))
- [Delegate Pattern에 대해서 - wannagohome 블로그](https://velog.io/@iwwuf7/Swift-Delegate-Pattern%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C)
- [[디자인 패턴]위임 패턴(Delegate Pattern)](https://june0122.github.io/2021/08/21/design-pattern-delegate/)

-  [코드의 재사용, 상속(Inheritance)보다 합성(Composition)을 사용해야 하는 이유 - MangKyu's Diary](https://mangkyu.tistory.com/199)

  
