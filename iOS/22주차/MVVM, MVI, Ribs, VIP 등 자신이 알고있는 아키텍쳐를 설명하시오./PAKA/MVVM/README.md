# MVVM
## 출처
[Kodeco mvc->mvvm 리팩토링](https://www.kodeco.com/6733535-ios-mvvm-tutorial-refactoring-from-mvc)
## MVVM을 왜 쓸까
### MVC의 한계
- MVC에서는 Controller가 Model과 소통하고, View의 업데이트를 담당합니다. 
  - View와 Controller는 분리가 힘들기 때문에 Model,View 관련 코드가 혼재되어 규모가 커질수록 Controller가 무거워집니다.
- 테스트 코드를 작성하기 쉽지 않다.
  - controller에 model,view 코드가 혼재되어 있고 강하게 연결되어 있어 각각을 분리시킨 테스트가 불가능합니다.
### MVVM의 구조
![mvvm architecture](https://koenig-media.raywenderlich.com/uploads/2019/12/MVVM-Diagram.png)

- view - controller - viewmodel - model의 구조
- controller와 model 사이에 viewmodel을 추가하여 controller에서 model과 관련된 로직을 걷어내 부담을 줄입니다.
- viewmodel은 controller과 비교하여 비즈니스 로직을 더 잘 표현할 수 있습니다. viewmodel의 역할은 다음과 같습니다.
  - view 입력을 받고 model의 데이터를 업데이트합니다.
  - 업데이트된 model의 데이터를 view로 전달합니다.
  - 업데이트 과정에서의 모델 데이터 formatting 작업을 수행합니다.
- viewmodel은 원활한 테스트를 위해 public으로 설정하고 view와의 완벽한 분리를 위해 UIKit을 import하지 않습니다.

## MVVM의 데이터 바인딩
- 단순히 viewmodel만 추가했다면 MVC 패턴 + viewmodel 관련 코드가 추가된 것뿐 오히려 복잡합니다.
- ios에서는 `데이터 바인딩`이라는 방법을 통해서 view와 model간 변화를 반영하고 간결한 로직 작성이 가능합니다.
### 방법
#### Key-Value Observer Pattern 
- objective-c에 기반한 방식으로, objective 런타임에서 앱을 실행시키게 됩니다.
#### 함수형 반응형 프로그래밍(FRP)
- RXSwift, Combine과 같은 라이브러리를 사용하는 방식입니다. 러닝커브가 높습니다☠️
#### Delegate Pattern
- notification을 통해 값의 변화를 감지합니다
#### Boxing(속성 감시자 사용)
- didSet, willSet을 활용하여 값이 변할 때 로직을 반영할 수 있습니다.

## MVC -> MVVM시 필요한 과정
### ViewController에서의 변화
- Model과 관련된 프로퍼티, 메서드들을 모두 viewmodel로 옮깁니다.
- view가 load된 후(ex : `viewDidLoad()`), viewmodel의 바인드 메서드를 호출하여 데이터 바인딩을 해줍니다.
### ViewModel, 필요에따라 Utility 클래스도 생성
#### viewmodel 구성요소
- view의 입력을 받아 업데이트 및 formatting할 model의 데이터를 프로퍼티로 선언
- 비즈니스 로직이 구현된 메서드들
#### Utility 클래스
- boxing의 경우, 변화를 관찰하고 값의 변화마다 비즈니스 로직을 적용해 줄 수 있는 객체입니다.
- RXSwift처럼 Observer 객체가 미리 구현되어 있는 경우도 있습니다.

<div><details>
  <summary>예시 코드</summary>
  
  ```swift
  final class Box<T> {

  typealias Listener = (T) -> Void
  var listener: Listener?

  var value: T {
    didSet {
      listener?(value)
    }
  }

  init(_ value: T) {
    self.value = value
  }

  func bind(listener: Listener?) {
    self.listener = listener
    listener?(value)
  }
}
```
  
  </details>
</div>

