# [2023.01.19] 프로토콜(Protocol)

### 프로토콜이란?

프로토콜은 약속이다.

약속을 안지키면 클래스, 구조체, 열거형 다 못만들어라는 애플의 강력한 의지

프로토콜이란 특정 객체가 갖춰어야 할 기능이나 속성에 대한 설계도!

바로 예로 보자구!

```swift
//약속
protocol Team { 
    var teamleader: String {get set}
    var teamMember1: String {get set}
    var teamMember2: String {get set}
    var teamMember3: String {get set}
    
    func coding()
    func playVideo()
}

//약속을 지키자
struct SwiftMaster: Team {
    var teamleader: String = "bible"
    var teamMember1: String = "leeds"
    var teamMember2: String = "paka"
    var teamMember3: String = "turing"
        
    func coding() {
        print("매일 백준 코딩을 합니다.")
    }
    
    func playVideo() {
        print("매일 엘런 인강을 듣습니다.")
    }
}

var ourteam: SwiftMaster = SwiftMaster()
ourteam.teamleader //bible
```

그런데.. 약속중에 지켜도되고 안지켜도 되는건..?  그래 코딩은 우리의 실세계를 녹이는 것이기때문이 이런 optional 약속이 필요할거야

@objc , @objc optional 키워드를 붙여주면 선택이 된다구!

```swift
//약속
@objc protocol Team { 
    var teamleader: String {get set}
    var teamMember1: String {get set}
    var teamMember2: String {get set}
    var teamMember3: String {get set}
    
    func coding()
    func playVideo()
		//출석은 선택이잖아!
		@objc optional func attendance() 
}

//약속을 지키자
struct SwiftMaster: Team {
    var teamleader: String = "bible"
    var teamMember1: String = "leeds"
    var teamMember2: String = "paka"
    var teamMember3: String = "turing"
        
    func coding() {
        print("매일 백준 코딩을 합니다.")
    }
    
    func playVideo() {
        print("매일 엘런 인강을 듣습니다.")
    }
}

var ourteam: SwiftMaster = SwiftMaster()
ourteam.teamleader //bible
```

실행했는데 오류가..? 아니 선택할 수 있다며! 

@objc 키워드르 붙이면 이 순간 우리 똑똑한 컴파일러는 objective-C에서도 사용가능한 객체로 인식해 

근데.. Objective-C 프로토콜은 오로지 클레스 전용에서만 채택할 수 있기때문에 → AnyObject가 채택~

프로토콜에서 **@objc 어노테이션이 붙은 프로토콜은 오직! 클래스만 구현할 수 있다고 기억**하자구

![%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8 58 43](https://user-images.githubusercontent.com/85090866/213462696-9cf9a442-1deb-4059-afdc-4af4982e63f2.png)

### 프로토콜 지켜야할 점!

- 프로토콜
1. 프로토콜에 내에 선언되는 프로퍼티는 var로 선언 → 아니면 오류나

---

- 프로토콜 채택
1. 저장프로퍼티는 let, var 상관없다.
2. 연산프로퍼티는 무조건 var 선언
3. 프로토콜 내에서 {get}으로 만들어서 사용하면 채택하는곳에서 {get} {get, set} 상관없다.
4. 프로토콜 내에서 {get, set}으로 들어서 사용하면 채택하는 곳에서 {get, set}만 사용가능

### 타입으로서의 프로토콜

프로토콜 자체로는 어떠한 기능을 못하는데 타입으로 쓴다고..? 

음.. 그니까 프로토콜을 상위클래스 타입으로 간주하여 사용하는거면 될까? → 즉.. 캐스팅해서 사용한다

1. 상수나 변수, 그리고 프로퍼티의 타입으로 프로토콜을 사용할 수 있다.
2. 함수, 메소드 또는 초기화 구문에서 매개변수 타입이나 반환 타입으로 프로토콜을 사용할 수 있다.
3. 배열이나 딕셔너리 타입으로 사용할 수 있다. 

어렵군… 앨런은 1급객체로 프로토콜, 함수, 클로저라고 하던데 !

예를 보면서 익혀보자구

```swift
protocol everydaySwift {
    func coding()
    func noCoding()
}

class Today: everydaySwift {
    var swiftSkill = 0
    
    func coding() {
        self.dayOff()
    }
    
    func noCoding() {
        self.busyDay()
    }
    
    // 쉬는날은 코딩해야지
    func dayOff() {
        swiftSkill += 1
    }
    
    // 너무 바쁜날이네.. 코딩 쉬자
    func busyDay() {
        swiftSkill += -1
    }
}

let studyYJ = Today()
studyYJ.coding() // +1
studyYJ.swiftSkill // swiftSkill = 1
```

![%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10 21 22](https://user-images.githubusercontent.com/85090866/213462818-6a0a3c1f-d429-4b09-a7bf-f69055a1edcb.png)


이건 당연히 모든 프로퍼티를 다 이용할 수 있잖아

프로퍼티를 타입으로 이용가능하다며 그러면 프로퍼티로 타입을 강제로 지정해주면?

```swift
let studyYJ: everydaySwift = Today()
```
![%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10 22 59](https://user-images.githubusercontent.com/85090866/213462891-77cfb239-743e-4895-aaac-ccaadd98292e.png)


실질적으로 studyYJ 상수에게 할당 된 객체는 everydaySwift타입이기 때문에 다른 메소드들은 컴파일러로부터 은닉이 되는구나.. 

오케이 그럼 상수와 변수의 타입으로 프로퍼티가 사용될 수 있고 이건 캐스팅의 개념이구나

함수는? 

```swift
protocol swiftMaster {
    var name: String { get }
}

struct swiftMember: swiftMaster {
    var name: String = "앨런"
    var name2: String = "영재"
    
}

func coding(master: swiftMaster) -> swiftMaster {
    return swiftMember.init()
}

let swiftPerson = swiftMember.init()
coding(master: swiftPerson).name //"앨런"
```

함수 역시 간단한데 프로토콜 타입으로 입력과 출력을 타입캐스팅해서 파라미터로 보내면 끝이야 :) 

결국 타입으로 선언된 name만 접근가능하고 name2는 접근이 불가해 !

### 프로토콜의 확장

말은 어렵게 확장이지만 개념은 쉽다.

- 프로토콜에서 기본적인 구현을해서 공통적으로 사용해보자

예를 들자면

```swift
protocol SwiftMaster {
    var name: String {get set}
    func coding()
}

class A: SwiftMaster {
    var name: String = "bible"
    func coding() {
        print("오늘 코딩 완료")
    }
}

class B: SwiftMaster {
    var name: String = "leeds"
    func coding() {
        print("오늘 코딩 완료")
    }
}
```

그래.. 약속은 지켜야 하니까 coding() 메서드를 다 구현했는데 이걸.. 클래스가 100개라면 다 써줘야해?? 

그렇기에 나온게 확장이야! 구현 = 확장 시켜서 우리는 채택만하면 기본 구현이 되있는거지

```swift
protocol SwiftMaster {
    var name: String {get set}
    func coding()
}

extension SwiftMaster {
    func coding(){
        print("코딩완료")
    }
}

class A: SwiftMaster {
    var name: String = "bible"
}

class B: SwiftMaster {
    var name: String = "leeds"
}

let Aclass = A()
Aclass.coding() // "코딩완료"
```

훨씬 깔끔한 코드를 할 수 있다는 것! 
메서드 뿐만 아니라 연산프로퍼티도 가능해 ^ㅡ^ 다만 저장 프로퍼티는 안된다는점
