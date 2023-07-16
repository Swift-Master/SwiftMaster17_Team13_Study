# AnyObject에 대해 설명하시오.

### TypeAlias, 모든 클래스가 암시적으로 준수하는 프로토콜

</br>

모든 클래스, 클래스 유형 또는 클래스 전용 프로토콜의 인스턴스에 대한 구체적인 유형으로 사용할 수 있습니다.

애플 공식문서 예시 - 해당 변수/상수의 타입이 클래스라면 메타타입, 심지어 Objective-C 클래스까지도 AnyObject로 선언가능합니다.

```swift
class FloatRef {
    let value: Float
    init(_ value: Float) {
        self.value = value
    }
}

let x = FloatRef(2.3)
let y: AnyObject = x
let z: AnyObject = FloatRef.self

let s: AnyObject = "브릿지 문자열" as NSString
print(s is NSString) // true

let v: AnyObject = 1 as NSNumber
print(type(of: v)) // __NSCFNumber
```

### objc 어노테이션(@objc)과 AnyObject

일반적인 경우, 선언시 AnyObject로 구체적 타입을 선언했다면 Swift가 컴파일 타임에 실제 인스턴스의 타입을 알 수 없기 때문에 그 프로퍼티나 메서드에 접근할 수 없습니다. -> 반드시 다운 캐스팅해야 접근 가능

그러나 @objc 메서드 또는 프로퍼티는 접근이 가능하게 만들고 Optional로 감싸진 결과를 반환합니다.

<p><image src = "https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/116094622/cbec0365-e042-4073-8bac-fa122e9e33b1"></image></p>
@objc 메서드가 아닌 경우 AnyObject타입에서 호출이 불가능합니다.
<p><image src = "https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/116094622/313b0179-e410-4ca8-a119-14b6955d7fc4"></image></p>

### AnyClass?

AnyClass = AnyObject.Type으로, 모든 클래스 타입(메타 타입)이 암묵적으로 준수하는 프로토콜입니다.

### 참고문헌

- [애플 공식문서 - Foundation - AnyObject](https://developer.apple.com/documentation/swift/anyobject)