# Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.


### **Equatable?**

타입끼리 비교(==)연산을 하기 위해서 필수적으로 채택해야 하는 프로토콜
<br>
＊ 프로토콜: 특정 역할을 하기 위한 메서드, 프로퍼티, 기타 요구사항 등의 청사진

⇒ **Equatable을 사용 시 구조체 / 클래스 / 열거형에서 비교연산이 가능하다.**

```swift
public protocol Equatable {
	static func == (lhs: Self, rhs: Self) -> Bool
}
```

<br><br>

**구조체 - 비교연산**

```swift
struct Music: Equatable {
	var title = ""
	var singer = ""
}

let music1 = Music.init()
let music2 = Music.init(title: "Movie", singer : "Tom misch")

music1 == music2   // false
```

프로토콜 내에 정의되어 있는 `==` 이란 메서드를 구현하지 않았지만,

열거형 내 모든 타입이 기본타입(title: String, singer: String)이라면 

직접 **메서드를 구현하지 않고 Equatable을 채택하는 것만으로도 사용 할 수 있다.**

<br><br>

**클래스 -  비교연산**

```swift
class Music: Equatable {     // Type 'Music' does not conform to protocol 'Equatable'
    var name = ""
    var age = 0
}
```

클래스는 구조체와 다르게 **Equatable 채택만으로는 자동구현이 불가능** 하기 때문에,

직접 `==` 메서드를 구현해야 한다.

```swift
class Music {
    var title: String
    var singer: String

init(title: String, singer: String) {
        self.title = title
        self.singer = singer
    }
}
 
extension Music: Equatable {
    static func == (lhs: Music, rhs: Music) -> Bool {
        return lhs.title == rhs.title && lhs.singer == rhs.singer
    }
}

let music1 = (t: "Ditto", s: "NewJeans")
let music2 = (t: "OMG", s: "NewJeans")

music1.s == music2.s   // true
```

<br><br>

**열거형 - 비교연산**

```swift
// 1번 예제

enum PublicTransport {
	case bus
}
 
let blue_bus = PublicTransport.bus
let green_bus = PublicTransport.bus

blue_bus == green_bus   // true
```

```swift
// 2번 예제 

enum PublicTransport : Equatable {
	case subway(name: String)
}
 
let line = PublicTransport.subway(name: "line 2")
let line2 = PublicTransport.subway(name: "line 5")
 
line == line2   // false
```

1번 예제인 연관값이 없는 열거형은 `Equatable` 을 채택하지도 않아도 자동 구현이 된다.

⇒ 연관값이 없으면 Equatable 안써도 됨

<br>

2번 예제인 연관값이 있는 열거형은 `Equatable` 채택만으로도 자동 구현이 된다. 

(구조체 비교연산과 같다. 채택은 필수지만 `==` 메서드 구현은 필수가 아님)

⇒ 연관값이 있으면 Equatable 써야함. 구현은 자동으로 됨

<br><br>

Equatable 정리 !

| 사용여부 | 구조체(struct) | 클래스(class) | 열거형(enum) |
| --- | --- | --- | --- |
| Equatable 채택 O<br>(==) 메서드 구현 X| ⭕ | ❌ | ⭕ |
| Equatable 채택 O<br>(==) 메서드 구현 O | ⭕ | ⭕ | ⭕ |
| Equatable 채택 X | ❌ | ❌ | 연관값 없는 열거형 ⭕<br>연관값 있는 열거형 ❌<br>→ Equatable 채택 시 사용 가능 |

⭕ = 사용 가능, ❌ = 사용 불가능

<br><br>

### **Hashable?**

"**정수 hash 값을 제공하는 타입**"으로 정의된 프로토콜이고,

자체적으로 `Equatable` 이라는 프로토콜을 준수하고 있다.



－ Hashable 프로토콜을 준수하는 모든 유형은 set, dictionary key로 사용
<br>
－ Hashable을 준수하고 있는 타입: String, Integer, floating-point, Boolean

```swift
// 코드는 dictionary 형태이고, 반드시 KeyType은 hash 가능한 타입이어야 한다.

dictionary<keyType, valueType>
```

hash값은 각각 타입의 인스턴스를 식별하는 값으로 유일성을 가지고 있어야 하고,

해당 값이 유일한 값인지 비교해야 되기 때문에 Equatable 프로토콜을 상속 받고 값을 비교한다.

<br><br>

### **정리**

❓ 그래서 왜 Hashable은 Equatable을 상속해야 되는걸까 ❓

⇒ hashValue는 고윳값이어야 하므로 고윳값인지 식별해 줄 수 있는 `==` 메서드가 필요하다.

`==` **메서드는 Equatable 프로토콜 안에 들어있기 때문에 Hashable이 Equatable을 상속해야 한다.**








<br><br><br><br><br>




Equatable, Hashable에 대해 이해가 잘 되지 않을 때 읽어보면 좋은 글 ..🙂
<br>
[https://medium.com/nerd-for-tech/equatable-hashable-and-comparable-d782449f6ce8](https://medium.com/nerd-for-tech/equatable-hashable-and-comparable-d782449f6ce8)
