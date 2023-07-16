# some 키워드에 대해 설명하시오.

some 키워드는 Swift 5.1에서 나온 새로운 기능으로 `불투명한 타입`을 나타낼 때 사용합니다.

```swift
protocol Shape {
    func describe() -> String
}

struct Square: Shape {
    func describe() -> String {
        return "I'm a square. My four sides have the same lengths."
    }
}

struct Circle: Shape {
    func describe() -> String {
        return "I'm a circle. I look like a perfectly round apple pie."
    }
}
```

불투명한 타입이란,
위와 같은 `Shape`라는 프로토콜을 채택한 여러 도형들이 있을 때

```swift
func makeShape() -> Shape {
  return Circle()
}
```

`makeShape()` 메서드는 `Shape`를 리턴하여 정확하게 어떤 도형을 리턴하는 것인지 타입이 불분명하게 됩니다.

이럴 때 `makeShape()`는 불투명한 타입(Opaque Return Type)이 됩니다.  
특정 구체적인 타입 대신 프로토콜을 반환 타입으로 사용하는 것을 의미합니다.

불투명한 타입인 것을 나타내주기 위해 `some` 키워드를 사용합니다.

```swift
func makeShape() -> some Shape {
  return Circle()
}
```

SwiftUI에서 계산 속성을 작성할 때 흔하게 사용하는 것 같습니다.

```swift
import SwiftUI

struct MyFirstView: View {
	var body: some View {
		Text(“Hello worl!”)
	}
}
```

### 출처
[Opaque Types and ‘some’ in Swift](https://www.appypie.com/app-builder/some-swift-opaque-types-how-to)