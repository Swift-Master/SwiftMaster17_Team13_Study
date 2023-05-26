# Retain Count 방식에 대해 설명하시오.

Retain Count는 객체가 생성(참조)되는 시점 +1 카운트 해서 현재 몇 개의 참조가 있는지 추적하고,  
참조가 존재하면 메모리에 계속 유지하게 하는 방식입니다.

지난 주 [ARC](https://github.com/Swift-Master/SwiftMaster17_Team13_Study/tree/main/iOS/15%EC%A3%BC%EC%B0%A8/ARC%EB%9E%80%20%EB%AC%B4%EC%97%87%EC%9D%B8%EC%A7%80%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./Bible)를 다뤘을 때의 썼던 코드를 다시 봅시다.
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
person1이 Person의 객체를 생성하면서 Person은 Retain Count 1을 갖게 됩니다.

```swift
person2 = person1
person3 = person1
```
이어서 참조를 2개 더 늘리면 Person은 Retain Count 3이 됩니다.

```swift
person1 = nil
person2 = nil
person3 = nil
```
모든 참조를 해제하게 되면 Retain Count는 0이 되어 메모리에서 해제하게 됩니다.

이렇게 참조 카운팅을 하는 Retain Count 방식은 Objective-C에서 Manual Retain-Release(MRR)이라는 방식을 구현하는데에 사용됐습니다.  
ARC는 OS X v10.6(약한 참조 불가) 및 v10.7(64비트 애플리케이션)의 Xcode 4.2, 그리고 iOS 4(약한 참조 불가) 및 iOS 5에서부터 사용할 수 있어서 이 이전으로는 MRR 방식만을 사용했습니다.

MRR 방식은 개발자가 직접 수동적으로 메모리를 관리하게 됩니다.  
메모리에 참조를 해야될 땐 **retain 메서드**를 호출하고, 참조 해제를 할 때는 **release 메서드**를 호출합니다.  
이 외에도 여러 메서드를 사용해서 메모리 관리를 수동적으로 했습니다.

ARC는 Retain Count 방식을 기반으로 자동적으로 메모리 관리를 하는 차이가 있습니다.

