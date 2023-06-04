# Strong 과 Weak 참조 방식에 대해 설명하시오.

## Strong(강한 참조)
두 개의 클래스 인스턴스가 서로를 강한 참조로 유지하여 메모리에서 해제되지 않는 상황을 말합니다.  
강한 참조가 형성되었을 때 메모리에서 제대로 해제하지 않는다면 메모리 누수가 발생할 수 있게 됩니다.

메모리 누수 혹은 강한 참조 사이클을 해결하기 위해서는 클래스 간의 참조 중 일부를 **약한 참조(weak reference)** 혹은 **무소유 참조(unowned reference)**로 정의해야 합니다.
```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) 소멸됨") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    var tenant: Person?
    deinit { print("Apartment \(unit) 소멸됨") }
}
```
Person과 Apartment라는 두 클래스가 있을 때 Apartment는 여러 개의 Person 인스턴스를 가지고 있고, Person 클래스는 Apartment 인스턴스에 대한 참조를 가지고 있다고 예를 들어봅시다. 이 때 Person 인스턴스와 Apartment 인스턴스 간에는 **강한 참조**가 형성됩니다.

두 클래스 모두 소멸자를 통해서 인스턴스들이 해제되는지 알 수 있습니다.

```swift
var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")
```
john 변수에는 Person 인스턴스를 할당하고, unit4A 변수에는 Apartment 인스턴스를 할당했습니다.

이 때는 john과 Person 사이에 **강한 참조**가 형성되고,  
unit4A와 Apartment 사이에 **강한 참조**가 형성됩니다.

<img width="683" alt="사진1" src="https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/59015538/51a6b357-c6cd-48c6-9f60-340a1b7404c4">

Person 인스턴스는 Reference Count 1,  
Apartment 인스턴스도 Reference Count 1로 각각 1씩 갖게 됩니다.

이제 john에게 아파트를 할당하고, unit4A 아파트에게 세입자 john을 할당해봅시다.
```swift
john!.apartment = unit4A
unit4A!.tenant = john
```
이렇게 두 클래스 인스턴스를 연결하면서 Person 인스턴스와 Apartment 인스턴스 사이에 **강한 참조**가 형성됩니다.

<img width="661" alt="사진2" src="https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/59015538/4ec74cba-0900-4542-83fd-39d566753fed">

서로를 참조하면서 Reference Count가 1씩 또 증가하니까  
Person 인스턴스는 Reference Count 2,  
Apartment 인스턴스 Reference Count 2가 됩니다.

이제 메모리에서 해제를 해봅시다.
```swift
john = nil
unit4A = nil
```
이 코드를 실행해도 소멸자는 실행되지 않습니다.  
아직 Reference Count가 각 인스턴스에 1씩 남아있는 강한 참조가 존재하기 때문에 ARC는 메모리에서 해제시키지 않습니다.

<img width="664" alt="사진3" src="https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/59015538/46353a87-8b9a-4ed2-baa1-85fca255aa11">

이 때 메모리 누수, 강한 참조 사이클이 발생하게 됩니다.

## Weak(약한 참조)

## Unowned(질문에는 없지만 추가해봅니다)