# property wrapper에 대해서 설명하시오.

### **Property Wrapper?**

- [Swift 5.1에서 추가된 기능](https://github.com/apple/swift-evolution/blob/main/proposals/0258-property-wrappers.md)
- 프로퍼티의 접근자(getter, setter)의 동작을 수정하거나 값 변환 등 추가적인 기능 제공
<br>⇒ 지역 변수에서만 사용 가능

<br>

언제 사용되고 왜 사용할까?

<br>

> When you use a property wrapper, you **write the management code once when you define the wrapper**, and then **reuse that management code by applying it to multiple properties.**
> 

⇒ 래퍼를 정의할 때 관리 코드를 작성한 뒤, 작성한 관리 코드를 여러 속성에 적용하여 재사용한다.

<br>

**사용이유**

1. 코드의 가독성과 간결성을 향상 시키기 위해
2. 코드의 안전성과 유지보수성을 향상시키기 위해

<br>

한줄로 정리하자면

property wrapper는 프로퍼티의 값의 저장과 접근을 커스터마이징 할 수 있도록 하는 기능이다 ! 👏

<br>

```swift
// property wrapper를 이용한 예제 1
// 각 단어의 처음 문자를 대문자로 변경(capitalized)

@propertyWrapper struct Capitalized {
    var wrappedValue: String {
        didSet { wrappedValue = wrappedValue.capitalized }
    }

    init(wrappedValue: String) {
        self.wrappedValue = wrappedValue.capitalized
    }
}

struct User {
    @Capitalized var firstName: String
    @Capitalized var lastName: String
}

// John Appleseed
var user = User(firstName: "john", lastName: "appleseed")

// John Sundell
user.lastName = "sundell"
```

```swift
// property wrapper를 이용한 예제 2
// n 이하의 수 구하기

@propertyWrapper
struct OrLess {
    private var number = 0
    var wrappedValue: Int {
        get { return number }
        set { number = min(newValue, 20) } // 20 이하의 수 설정
    }
    
    init(wrappedValue: Int) {
        number = min(wrappedValue, 20)
    }
}

struct Rectangle {
    @OrLess var height: Int = 2
    @OrLess var width: Int = 200
}

var rectangle = Rectangle()
print(rectangle.height) // 2
print(rectangle.width) // 20
```

<br><br><br>

📖 **참고**

---

[https://www.swiftbysundell.com/articles/property-wrappers-in-swift/#a-propertys-properties](https://www.swiftbysundell.com/articles/property-wrappers-in-swift/#a-propertys-properties)
