# 애플 공식 문서(Choosing Between Structures and Classes)

[https://developer.apple.com/documentation/swift/choosing-between-structures-and-classes](https://developer.apple.com/documentation/swift/choosing-between-structures-and-classes)

동작을 모델링하고 데이터를 저장하는 좋은 선택 - 항상 클래스와 둘중 뭘 사용할지 고민됨.

공식문서의 추천사항

- 기본적으로 구조체를 써라(!)
- Object-C와의 상호 작용이 필요하거나 다루는 데이터의 본질(?)을 다룰 때 클래스를 사용하자
- 구현을 공유함으로써 동작을 채택하는 프로토콜과 함께 구조체를 쓰자

### 기본적으로 구조체를 써라!

- 구조체는 공통된 데이터를 많이 가지고 있고, 다른 언어에서는 클래스에 대해 제한된 다양한 특징을 가지고 있음.
    - 저장 속성, 계산 속성, 메서드
- 기본 구현을 통해 동작을 얻는 프로토콜을 채택할 수 있다.
- 스위프트 표준 라이브러리와 Foundation은 자주 쓰는 타입들(Int, String, Array, Dictionary…)을 위해 구조체를 사용한다.
- 구조체를 사용하면 앱 전체 상태를 고려할 필요 없이 코드 일부를 더 쉽게 추론할 수 있다
    - 참조타입인 클래스와 달리 구조체는 값타입이기 때문에 app flow에 변경 사항을 의도적으로 전달하지 않는 이상 구조체의 로컬 변경 사항은 앱의 나머지 부분에 표시되지 않는다
    - 이를 통해 코드에 의한 인스턴스 변경은 보이지 않는 함수 호출에 의한 것이 아니라 명시적 변경에 의한 것 임을 확신할 수 있다?

## 클래스를 쓰는게 더 권장될 때

### objcetive-c와의 상호 운용이 필요해!

[https://developer.apple.com/documentation/swift/using-imported-c-structs-and-unions-in-swift](https://developer.apple.com/documentation/swift/using-imported-c-structs-and-unions-in-swift)

- Objective-C로 설계된 클래스를 swift로 import해야 될 경우에는 class를 써줘야한다.

</br>

### 참조하는 값인 ID(Identifier)를 컨트롤할 필요가 있어!

- 클래스는 참조 타입이기 때문에 ID라는 개념이 내장되어 있고 이것은 서로 다른 클래스 인스턴스가 각각의 저장 속성에 대해 같은 값을 가질 때 항등 연산자(===)로 비교시 false를 반환하도록 만듭니다. 
- 앱 전체에서 클래스 인스턴스를 공유할 때 변경 사항이 해당 인스턴스에 대한 참조를 보유하는 코드의 모든 부분에 반영됩니다. 인스턴스가 이러한 종류의 ID를 가져야 하는 경우 클래스를 사용하십시오. 
- 주로 공유 하드웨어 중개자, 파일 관리자, 네트워크 연결 기능을 하는 경우 클래스를 사용합니다.
    
 <h2> 결론 : 변수 하나의 변경사항이 앱 전체에 반영되야 하는 경우에는 클래스를 쓰자</h2> 
    
</br>
*컨트롤하지 않는 id를 가진 엔티티의 정보를 포함한 데이터를 모델링할 때는 구조체를 써라

- 예를 들어 원격 데이터베이스를 참조하는 앱에서 인스턴스의 ID는 외부 엔터티가 완전히 소유하고 식별자로 통신할 수 있습니다. 앱 모델의 일관성이 서버에 저장되어 있는 경우 레코드를 식별자가 있는 구조로 모델링할 수 있습니다.

```swift
struct PenPalRecord {
    let myID: Int
    var myNickname: String
    var recommendedPenPalID: Int
}

var myRecord = try JSONDecoder().decode(PenPalRecord.self, from: jsonResponse)
//
```

PenPalRecord와 같은 모델 유형에 대한 로컬 변경이 유용합니다. 예를 들어 앱은 사용자 피드백에 대한 응답으로 여러 펜팔을 추천할 수 있습니다. PenPalRecord 구조는 기본 데이터베이스 레코드의 ID를 제어하지 않기 때문에 로컬 PenPalRecord 인스턴스에 대한 변경 사항으로 인해 데이터베이스의 값이 실수로 변경될 위험이 없습니다.

앱의 다른 부분이 myNickname을 변경하고 변경 요청을 다시 서버에 제출하면 가장 최근에 거부된 펜팔 추천이 변경으로 인해 실수로 선택되지 않습니다. myID 속성은 상수로 선언되기 때문에 로컬에서 변경할 수 없습니다. 결과적으로 데이터베이스에 대한 요청은 실수로 잘못된 레코드를 변경하지 않습니다.

## 모델 상속과 공유 동작을 위해 구조체와 프로토콜을 사용하자

- 구조체는 상속을 지원하지 않지만, 프로토콜 상속과 구조체를 함께 사용하면 클래스 상속으로 만들 수 있는 계층 구조를 만들 수 있습니다.
- 프로토콜은 클래스의 상속과 달리 구조체, 열거형, 클래스 모두 상속 가능하기 때문에 훨씬 범용성이 좋습니다.

- `` 결론 : 상속을 통한 프로젝트 계층 구조를 설계하고 싶다면 프로토콜 상속으로 구조를 설계하고 구조체에서 해당 프로토콜을 채택하는 것이 바람직하다. ``
