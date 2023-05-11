# ARC란 무엇인지 설명하시오.

Swift는 앱의 메모리 사용을 추적하고 관리하기 위해 `ARC(자동 참조 카운팅, Automatic Reference Counting)`을 사용합니다.  
대부분의 경우에서는 메모리 관리가 알아서 작동되기 때문에 개발자가 메모리 관리에 대해서 생각할 필요는 없습니다.

ARC는 클래스의 인스턴스가 더 이상 필요하지 않을 때 해당 인스턴스가 사용한 메모리를 자동으로 해제합니다.  
=> **참조** 카운팅이기 때문에 `클래스 인스턴스`에만 적용됩니다. 값 타입인 구조체와 열거형은 적용되지 않습니다.

Swift에서 새로운 클래스 인스턴스를 생성할 때마다 ARC는 해당 인스턴스에 대한 정보를 저장하기 위해 메모리를 할당하고,  
인스턴스가 더 이상 필요하지 않은 경우 ARC는 해당 인스턴스에 사용된 메모리를 해제하여 메모리 공간을 차지하지 않도록 합니다.

ARC는 클래스 인스턴스를 사용하는 변수, 상수, 프로퍼티의 개수를 추적하여 인스턴스가 필요한 동안에는 메모리에서 해제되지 않도록 합니다.  
만약 인스턴스를 아직 사용하고 있는 중에 ARC가 해당 인스턴스를 해제하게 된다면 인스턴스의 프로퍼티나 메서드에 접근할 수 없게 되고 앱이 충동 날 가능성이 생깁니다.

따라서 클래스 인스턴스를 변수, 상수, 프로퍼티에 할당할 때마다 해당 변수, 상수, 프로퍼티는 인스턴스에 대한 **강한 참조**를 가지게 됩니다.  
이 강한 참조는 해당 인스턴스를 계속 유지하게 해서 인스턴스가 필요한 동안에는 메모리에서 해제되지 않도록 해줍니다.

## 예제 코드
```swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
        print("\(name) 생성됨")
    }
    deinit {
        print("\(name) 소멸됨")
    }
}

// nil값을 자동으로 초기화하여 Person 인스턴스를 참조하지 않은 상태
var person1: Person?
var person2: Person?
var person3: Person?

// 인스턴스 참조
person1 = Person(name: "Bible")
// Console Print : Bible 생성됨
```
Person 클래스의 인스턴스가 초기화 될 때 생성자가 호출되어 "생성됨"을 출력하고,  
인스턴스가 해제될 때 "소멸됨"을 호출하는 소멸자를 가지고 있습니다.

person1 변수에 Person 인스턴스가 할당되면서 강한 참조를 형성하게 됩니다.  
적어도 하나 이상의 강한 참조가 생기면 ARC는 Person 인스턴스가 메모리에 유지되고 해제되지 않도록 합니다.

만약 동일한 인스턴스를 2개의 새로운 변수에 할당하게 된다면 이 인스턴스에는 2개의 추가적인 강한 참조가 생성됩니다.
```swift
person2 = person1
person3 = person1
```
Person 인스턴스는 3개의 강한 참조가 생기게 됩니다.

처음에 생성했던 person1과 person2에 nil을 할당하게 되면 두 개의 참조가 끊기고 하나의 참조만 남게 됩니다.
```swift
person1 = nil
person2 = nil
```
아직 하나의 참조가 남은 상태기 때문에 ARC는 남은 참조가 끊길 때까지 Person 인스턴스를 해제하지 않습니다.

```swift
person3 = nil
// Console Print : Bible 소멸됨
```
person3에 nil을 할당하는 시점에서 모든 참조가 끊기게 되고,  
ARC는 Person 인스턴스가 더 이상 사용하지 않게되는 것을 알게 되어 메모리에서 해제하게 됩니다.

## 출처
[Swift Documentation - Automatic Reference Counting](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/automaticreferencecounting/)