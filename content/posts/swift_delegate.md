

+++

title = "스위프트의 Delegate Pattern 정리(작성중)"
tags = ["swift"]
categories = ["Swift&iOS"]
date = "2022-02-19"

+++

​    

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
To use image picker controller, you must provide a delegate that conforms to the UIImagePickerControllerDelegate procotol.
```

실제로 __`UIViewController`__ 의 내부 구조를 보면 프로퍼티로 __`delegate`__ 라는 프로퍼티를 가지고 있는 것을 확인할 수 있는데, 공식문서에서는 이것이 위에서 언급하는 __`delegate object`__ 에 해당한다.

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

위의 코드는 이러한 지침을 적용해서 작성한 간단한 ViewController 클래스 코드이다. 작성한  __*imagePicker.delegate = self*__  는 클래스가 가지고 있는 UIImagePickerController 인스턴스의 __`delegate object`__ 가 자기 자신(MyViewController)이 될 것임을 의미한다. 또한 이를 위해 해당 클래스가  __`UIImageControllerDelegate`__ 을 채택하고 있음을 명시하였다.

이후 선택 버튼(selectButton)을 누르면 미리 지정해둔 __`selectedButtonTouched`__ 액션 메소드가 실행되면서 화면에 사진선태

​    

### 1-2. UIImagePickerControllerDelegate

그렇다면 채택한 __`UIImageControllerDelegate`__ 프로토콜은 구체적으로 무슨 역할을 하는 걸까?

 [공식문서](https://developer.apple.com/documentation/uikit/uiimagepickercontrollerdelegate) 에서는 해당 프로토콜에 대해 아래와 같이 정의하고 있다.

```
A set of methods that your delegate object must implement to interact with the image picker interface.
```

간단히 말해 image picker 인터페이스와 관련된 __사용자 동작의 결과를 처리하는 메소드들을 담고 있는 프로토콜로 이해할 수 있다.__ 또한 동작의 결과를 처리하는 메소드들은 결과적으로 프로토콜을 채택한 __`delegate object`__ 내부에서 구현되야 할 것이다.

​    

```swift
class MyViewController: UIViewController, UIImagePickerControllerDelegate {
  /*
      나머지 부분 생략
  */
  
  //UIImagePickerControllerDelegate의 필요 메소드 구현
  func imagePickerController(_ picker: UIImagePickerController, 
                             didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
    
    //라이브러리로부터 가져온 이미지(UIImage)
    let selectedImage = info[UIImagePickerController.InfoKey.originalImage] as? UIImage
    
    //뷰컨트로러의 imageView의 image로 가져온 이미지를 할당
    self.imageView.image = selectedImage
    
    //화면에 띄웠던 UIImagePickerController의 View를 해제
    picker.dismiss(animated: true, completion: nil)
  }
}
```

위의 코드의 __`imagePickerController()`__ 메소드가 바로 앞서 언급한 사용자 동작의 결과를 처리하는 메소드가 된다. 공식문서에서는 해당 메소드에 대해 *'Tells the delegate that the user picked a still image or movie.'* 와 같이 __`delegate object`__ 에 사용자의 이미지 선택의 결과를 전달하는 것으로 설명하고 있다.

​    

위와 같이 작성한 후 실제로 __`selectButtonTouched`__ 메소드를 실행시키면 앨범에서 사진을 선택하는 창이 나타나면서  __`imagePickerController`__ 메소드의 내부 로직이 실행되는 것을 확인할 수 있다. 

<img src="https://user-images.githubusercontent.com/68586291/154850655-7b768528-916a-453c-bbe5-75cf5ae4033a.png" alt="image" style="zoom:80%;"/>

위의 그림과 같이 정리해보면 우선  __`UIImpagePickerController`__ 은 사용자의 동작 결과를 처리하는 일을 __`MyViewController`__ 에 위임(delegate) 하였고, 해당 뷰컨트롤러는 __`UIImagePickerControllerDelegate`__ 를 채택함으로써 __`delegate object`__ 로서 기능하게 된다. 이후 UIImagePickerController은 사용자 동작의 결과를 __`UIImagePickerControllerDelegate`__ 를 거쳐 __`MyViewController`__ 에 전달하게 되며, __`MyViewController`__ 내부에서 __`UIImagePickerControllerDelegate`__ 를 __`imagePickerController()`__ 과 같은 채택하면서 구현한 메소드가 실행될 것이다.  

​    

별 것 아닌 것처럼 보이지만, 실제로 클래스 내부에서 __`imagePickerController`__ __메소드를 직접적으로 호출하는 로직을 명시하지 않았음에도 불구하고 메소드 내부의 로직이 자동으로 실행된 것을 확인할 수 있다.__

​        

## Reference

- [UIImagePickerControllerDelegate - 애플 공식문서](https://developer.apple.com/documentation/uikit/uiimagepickercontrollerdelegate)

​    
