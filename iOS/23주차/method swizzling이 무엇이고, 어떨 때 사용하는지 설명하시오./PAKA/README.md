# method swizzling이 무엇이고, 어떨 때 사용하는지 설명하시오.
## 출처
- [Method swizzling in iOS Swift - medium](https://abhimuralidharan.medium.com/method-swizzling-in-ios-swift-1f38edaf984f#:~:text=What%20is%20method%20swizzling%3F,an%20Objective%2DC%20runtime%20feature.)
- [Method Swizzling에 대해 알아보자 - 개발자 소들이님](https://babbab2.tistory.com/76)
- [Method Swizzling 응용 deinit 로그 찍기 - 김종권님 블로그](https://ios-development.tistory.com/911)

## 정의
iOS의 언어인 Swift, Objective-C 모두 런타임에 코드가 확정되는 'dynamic lanuguage'입니다.

method Swizzling은 번역 시 메서드 뒤섞기로, 말그대로 코드로 작성된 메서드를 런타임에 다른 메서드로 바꿔치기 하는 것입니다.

메서드를 수정할 수 없는 상태(시스템, 라이브러리의 블랙박스 메서드들)일 때 유용하게 사용가능합니다.

## 사용법
제약조건 : method swizzling은 Objective-C 런타임의 기능이기 때문에 swift, Objective-C 모두 사용가능하나 `@objc dynamic` 어노테이션을 대상 메서드 앞에 붙혀야 합니다.

```swift
import UIKit

class ViewController: UIViewController {

  @objc dynamic private func original() {
    print("원본 메서드")
  }
}
```
`class_getInstanceMethod` 함수로 해당 메서드의 인스턴스를 획득하고 `method_exchangeImplemtations` 함수를 통해 런타임 때 원본 메서드를 교체합니다.
```swift
extension ViewController {
  class func swizzleMethod() {
    guard
      let originalMethod = class_getInstanceMethod(Self.self, #selector(Self.original)),
      let swizzledMethod = class_getInstanceMethod(Self.self, #selector(Self.dynamic))
    else { return }
    method_exchangeImplementations(originalMethod, swizzledMethod)
  }
  
  @objc private func dynamic() {
    print("교체할 메서드")
  }
}
```
## 주의할 점
시스템에서 정의된 viewDidLoad와 같은 메서드는 swizzling 후 교체된 메서드뿐만 아니라 기존 메서드도 함께 호출됩니다.

최신 iOS 버전이 출시되면 스위즐링이 실패할 가능성이 있어 항상 확인해야 합니다.

하위 클래스 내에서 스위즐링 시 예상치 못한 변경이 발생할 수 있습니다.

