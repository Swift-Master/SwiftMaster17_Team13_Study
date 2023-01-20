# Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오

### **Delegate Pattern**

위임자를 갖고 있는 객체가 다른 객체에게 자신의 일을 위임하는 형태의 디자인 패턴이다.<br>
→ delegate 패턴을 통해 특정 기능을 위임하는 것이 가능

<br>

delegate를 제대로 사용하기 위해서는 아래 내용들을 이해하고 있어야 함

```
1. 프로토콜 (Protocol) : 대리자가 수신자에게 전달할 내용의 규칙
2. 수신자 (Receiver) : 대리자가 특정 기능을 수행 후 전달 받을 대상
3. 대리자 (Delegate) : 수신자를 대신하여 처리할 대리자
```

<br><br>

## **프로토콜**이란?
⇒ 특정 역할을 하기 위한 메소드, 프로퍼티, 기타 요구사항 등의 청사진(Blueprint)

<br>

쉽게 설명하자면…

<br>

`밴드` 의 기본은 
`기타`, `드럼`, `피아노`, `보컬` 이 필수적으로 있어야 한다.

또 `밴드` 에 대한 또 다른 요구사항 중 하나는 `연주` 이다.

<br>

<div markdown="1">

| Band |      |      |      |
| ------ |------ | ------ | ------ |
| 보컬 | 기타 | 피아노 | 드럼 |
play() 


</div>

<br>

`연주`라는 `play()` 까지 추가함으로써, 
밴드라는 것에 대한 가이드를 만들었다.

<br>

위 내용을 프로그래밍 적으로 생각해보자면

`밴드` 를 구성할 때 필요한 속성 `기타`, `드럼`, `피아노`, `보컬` 은 ***프로퍼티***로, 

`play()` 라는 연주에 대한 행위는 ***메서드***로 만들 수 있다.

<br>

따라서 <U>***프로토콜***</U>이란 것은 이런 ***프로퍼티***는 꼭 필요하고 이런 ***메서드***는 꼭 필요해요!

라고 해당 기능에 **필요한 요구사항을 선언**해두는 것이다.


<br><br>

## **프로토콜 요구 사항**

```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }    // { get set } => 읽기, 쓰기 가능 프로퍼티
    var doesNotNeedToBeSettable: Int { get }   // { get } => 읽기 전용 프로퍼티
    static var someTypeProperty: Int { get set } // typeProperty
}
```

- 프로토콜은   protocol  키워드를 사용
- 프로토콜에서는 프로퍼티의 이름과 타입 그리고 gettable, settable을 명시
- 필수 프로퍼티는 항상  var 로 선언해야 하고, 타입 프로퍼티는  static 키워드를 적어 선언
    

<br><br>


## **프로토콜 초기 구현**
```swift
// 프로토콜 정의
protocol Person {
    var name: String? { get }
    var age: Int? { get }
    
    func getName() -> String?
    func getAge() -> Int?
}
 
// 프로토콜 기능 확장
extension Person {
    func getName() -> String? {
        return self.name
    }
    
    func getAge() -> Int? {
        return self.age
    }
}

// Student 구조체가 Person 프로토콜 채택
struct Student: Person {
    var name: String?
    var age: Int?
    
    init(name: String?, age: Int?) {
        self.name = name
        self.age = age
    }
}

// ① Student 구조체 초기화
// ② .getName(프로토콜 함수명) 메소드를 호출하면 이름이 출력됨
let profile = Student(name: "je", age: 12)
print(profile.getName()) // je
```

<br><br>

## **실생활 예시 코드 (파티 준비)**
```swift
protocol PrepareParty: class {
	func prepareFood()
	func prepareSong()
}

class PartyDirector {
	weak var delegate: PrepareParty?

    func order() {
        self.delegate?.prepareFood()
        self.delegate?.prepareSong()
    }
}

class FirstPartyWorker: PrepareParty {
	init(director: PartyDirector) {
		director.delegate = self // PartyDirector의 delegate를 연결해주는 코드
	}

    func prepareFood() {
        print("First worker prepared pizza")
    }

    func prepareSong() {
        print("First worker prepared NewJeans - OMG")
    }
}

class SecondPartyWorker: PrepareParty {
    init(director: PartyDirector) {
        director.delegate = self // PartyDirector의 delegate를 연결해주는 코드
    }

    func prepareFood() {
        print("Second worker prepared chicken")
    }

    func prepareSong() {
        print("Second worker prepared NewJeans - Ditto")
    }
}

let je = PartyDirector()

let first = FirstPartyWorker(director: je)
je.order()
// First worker prepared pizza
// First worker prepared NewJeans - OMG

let second = SecondPartyWorker(director: je)
je.order()
// Second worker prepared chicken
// Second worker prepared NewJeans - Ditto
```

<br><br>

<details>
<summary style="font-size: 24px; font-weight: bold;">활용 사용 예시</summary>

<div markdown="1"><br>

delegate 패턴을 사용하는 가장 흔한 경우는 두 ViewController 간에 데이터를 전달 할 때다. 

사용자 입력 등의 이벤트를 받은 ViewController와 그 결과를 처리해줘야하는

ViewController가 서로 다른 경우

<br>

>ex) 사용자가 프로필 수정창 에서 **이름, 전화번호, 주소** 등을 입력하고
>확인 버튼을 눌러<br>이전 화면으로 돌아갔을 때 <U>입력 받은 정보(이름, 전화번호, 주소)를 
>이전 화면으로 <br>전달하여 보여줘야 하는 경우</U>들을 말한다.

</div>
</details>

<br>

<details>
<summary style="font-size: 24px; font-weight: bold;">사용 이유</summary>

<div markdown="1"><br>


- 코드 재사용, 유지보수가 쉽다.
- 작업을 전달할 때 공통된 부분을 제외하고 처리해야 하는 부분만 전달하여 처리할 수 있다.

</div>
</details>

<br>

<details>
<summary style="font-size: 24px; font-weight: bold;">장·단점</summary>

<div markdown="1"><br>

```swift
> 🤭 장점

- 매우 엄격한 Syntax로 인해 프로토콜에 필요한 메서드들이 명확하게 명시된다.
- 컴파일 시 경고나 에러가 떠서 프로토콜의 구현되지 않은 메소드들을 알려준다
- 로직의 흐름을 따라가기 쉽다
- 프로토콜 메소드로 알려주는 것 뿐만 아니라 정보를 받을 수 있다.
- 커뮤니케이션 과정을 유지하고 모니터링하는 제 3의 객체가 필요없다. 
  (NotificationCenter 같은 외부 객체)
- 프로토콜이 컨트롤러의 범위 안에서 정의된다.
```
```swift
> 😔 단점

- 많은 줄의 코드가 필요하다
- delegate 설정에 nil이 들어가지 않게 주의해야 한다. crash를 일으킬 수 있다.
- 많은 객체들에게 이벤트를 알려주는 것이 어렵고 비효율적이다.
```

</div>
</details>

<br><br><br><br>

### 📖 **참고**

---

[https://velog.io/@zooneon/Delegate-패턴이란-무엇일까#-delegate-패턴이-대체-뭐야](https://velog.io/@zooneon/Delegate-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C#-delegate-%ED%8C%A8%ED%84%B4%EC%9D%B4-%EB%8C%80%EC%B2%B4-%EB%AD%90%EC%95%BC)

[https://babbab2.tistory.com/174](https://babbab2.tistory.com/174)

