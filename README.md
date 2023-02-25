# SwiftMaster17_Team13_Study




## 질문 리스트

### 현재 진행중
<details><summary>IOS 면접에 나올 질문들 총 정리
</summary>

 - [Jercy님 IOS 면접질문 레포](https://github.com/JeaSungLEE/iOSInterviewquestions)   

## iOS
- Bounds 와 Frame 의 차이점을 설명하시오.
- 실제 디바이스가 없을 경우 개발 환경에서 할 수 있는 것과 없는 것을 설명하시오.
- 앱의 콘텐츠나 데이터 자체를 저장/보관하는 특별한 객체를 무엇이라고 하는가?
- 앱 화면의 콘텐츠를 표시하는 로직과 관리를 담당하는 객체를 무엇이라고 하는가?
- App thinning에 대해서 설명하시오.
###
- 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?
- @Main에 대해서 설명하시오.
- 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요?
- 상태 변화에 따라 다른 동작을 처리하기 위한 앱델리게이트 메서드들을 설명하시오.
- 앱이 In-Active 상태가 되는 시나리오를 설명하시오.
- scene delegate에 대해 설명하시오.
- UIApplication 객체의 컨트롤러 역할은 어디에 구현해야 하는가?
- App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오.
###
- NSOperationQueue 와 GCD Queue 의 차이점을 설명하시오.
- GCD API 동작 방식과 필요성에 대해 설명하시오.
- Global DispatchQueue 의 Qos 에는 어떤 종류가 있는지, 각각 어떤 의미인지 설명하시오.
###
- iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?
- Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명하시오.
- Delegate란 무엇인지 설명하고, retain 되는지 안되는지 그 이유를 함께 설명하시오.
- NotificationCenter 동작 방식과 활용 방안에 대해 설명하시오.
- UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?
- App Bundle의 구조와 역할에 대해 설명하시오.
- 모든 View Controller 객체의 상위 클래스는 무엇이고 그 역할은 무엇인가?
- 자신만의 Custom View를 만들려면 어떻게 해야하는지 설명하시오.
- View 객체에 대해 설명하시오.
- UIView 에서 Layer 객체는 무엇이고 어떤 역할을 담당하는지 설명하시오.
- UIWindow 객체의 역할은 무엇인가?
- UINavigationController 의 역할이 무엇인지 설명하시오.
- TableView를 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오.
- 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오.
- setNeedsLayout와 setNeedsDisplay의 차이에 대해 설명하시오.
- stackView의 장점과 단점에 대해서 설명하시오.
###
- NSCache와 딕셔너리로 캐시를 구성했을때의 차이를 설명하시오.
- URLSession에 대해서 설명하시오.
- prepareForReuse에 대해서 설명하시오.
- 다크모드를 지원하는 방법에 대해 설명하시오.
- <s>ViewController의 생명주기를 설명하시오.</s>
- <s>TableView와 CollectionView의 차이점을 설명하시오.</s>

## Autolayout
- 오토레이아웃을 코드로 작성하는 방법은 무엇인가? (3가지)
- hugging, resistance에 대해서 설명하시오.
- Intrinsic Size에 대해서 설명하시오.
- 스토리보드를 이용했을때의 장단점을 설명하시오.
- Safearea에 대해서 설명하시오.
- Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.

## Swift
- <s>struct와 class와 enum의 차이를 설명하시오.</s>
- <s>class의 성능을 향상 시킬수 있는 방법들을 나열해보시오.</s>
- <s>Copy On Write는 어떤 방식으로 동작하는지 설명하시오.</s>
- Convenience init에 대해 설명하시오.
- AnyObject에 대해 설명하시오.
- <s>Optional 이란 무엇인지 설명하시오.</s>
- <s>Struct 가 무엇이고 어떻게 사용하는지 설명하시오.</s>
- <s>Subscripts에 대해 설명하시오.</s>
- <s>String은 왜 subscript로 접근이 안되는지 설명하시오.</s>
- <s>instance 메서드와 class 메서드의 차이점을 설명하시오.</s>
- <s>class 메서드와 static 메서드의 차이점을 설명하시오.</s>
- <s>Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.</s>
- <s>Singleton 패턴을 활용하는 경우를 예를 들어 설명하시오.</s>
- <s>KVO 동작 방식에 대해 설명하시오.</s>
- <s>Delegates와 Notification 방식의 차이점에 대해 설명하시오.</s>
- <s>멀티 쓰레드로 동작하는 앱을 작성하고 싶을 때 고려할 수 있는 방식들을 설명하시오.</s>
- <s>MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.</s>
- <s>프로토콜이란 무엇인지 설명하시오.</s>
- Protocol Oriented Programming과 Object Oriented Programming의 차이점을 설명하시오.
- <s>Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.</s>
- mutating 키워드에 대해 설명하시오.
- 탈출 클로저에 대하여 설명하시오.
- <s>Extension에 대해 설명하시오.</s>
- <s>Extension 내부에서 함수를 override할 수 있는지 설명하시오.</s>
- <s>접근 제어자의 종류엔 어떤게 있는지 설명하시오.</s>
- defer란 무엇인지 설명하시오.
- defer가 호출되는 순서는 어떻게 되고, defer가 호출되지 않는 경우를 설명하시오.
- <s>property wrapper에 대해서 설명하시오.</s>
- <s>Generic에 대해 설명하시오.</s>
- some 키워드에 대해 설명하시오.
- <s>Result타입에 대해 설명하시오.</s>
- <s>Codable에 대하여 설명하시오.</s>
- <s>Closure에 대하여 설명하시오.</s>
- <s>Closure와 함수와의 관계에 대해 설명하시오.</s>

## ARC
- ARC란 무엇인지 설명하시오.
- Retain Count 방식에 대해 설명하시오.
- Strong 과 Weak 참조 방식에 대해 설명하시오.
- 순환 참조에 대하여 설명하시오.
- 강한 순환 참조 (Strong Reference Cycle) 는 어떤 경우에 발생하는지 설명하시오.

## Functional Programming
- 순수함수란 무엇인지 설명하시오.
- 함수형 프로그래밍이 무엇인지 설명하시오.
- 고차 함수가 무엇인지 설명하시오.
- <s>Swift Standard Library의 map, filter, reduce, compactMap, flatMap에 대하여 설명하시오.</s>

## Architecture
- MVVM, MVI, Ribs, VIP 등 자신이 알고있는 아키텍쳐를 설명하시오.
- 의존성 주입에 대하여 설명하시오.

## SwiftUI
- @State에 대해서 설명하시오.

## Combine
- PassthroughSubject에 대해서 설명하시오
- @Published에 대해서 설명하시오
- AnyCancellable에 대해서 설명하시오
- sink에 대해서 설명하시오
- throttle과 debounce의 차이점을 설명하시오.
- Data를 Binding 하는 방법에 대해서 설명하시오.

# Optional
아래부터는 추가로 공부를 하면 좋을 내용들입니다.

Objective-c나 rx는 회사, 팀마다 사용하는곳이 차이가있고 신입이나 주니어기준으로 필수라고 여겨지지않기에 옵셔널에 추가하였습니다.

## Rx
- Reactive Programming이 무엇인지 설명하시오.
- RxSwift를 왜 사용하는지 설명하시오.
- RxSwift의 단점을 설명하시오.
- RxSwift에서 Hot Observable과 Cold Observable의 차이를 설명하시오.
- Subject의 종류와 차이점에 대해 설명하시오.
- Subject와 Driver의 차이를 설명하시오.
- Single, Completable, Maybe의 차이점에 대해 설명하고, 언제 적용하면 좋을지 설명하시오.

## MRC
- ARC 대신 Manual Reference Count 방식으로 구현할 때 꼭 사용해야 하는 메서드들을 쓰고 역할을 설명하시오.
- retain 과 assign 의 차이점을 설명하시오.
- 특정 객체를 autorelease 하기 위해 필요한 사항과 과정을 설명하시오.
- Autorelease Pool을 사용해야 하는 상황을 두 가지 이상 예로 들어 설명하시오. 
- 다음 코드를 실행하면 어떤 일이 발생할까 추측해서 설명하시오.
Ball *ball = [[[[Ball alloc] init] autorelease] autorelease];

## Advanced
- method swizzling이 무엇이고, 어떨 때 사용하는지 설명하시오.
- NSCoder 클래스는 어떤 상황에서 어떻게 써야 하는지 설명하시오.
- Responder Chain 구조에 대해 설명하고, First Responder 역할에 대해 설명하시오.
- NSObject부터 UIButton 까지 상속 과정의 계층과 역할을 설명하시오.
- shallow copy와 deep copy의 차이점을 설명하시오.
- Push Notification 방식에 대해 설명하시오.
- Foundation 과 Core Foundation 프레임워크의 차이점을 설명하시오.
- NSURLConnection 에서 사용하는 Delegate 메서드들에 대해 설명하시오.
- Synchronous 방식과 Asynchronous 방식으로 URL Connection을 처리할 경우의 장단점을 비교하시오.
- Plist 파일 구조와 Plist 파일에 저장된 데이터를 다루기 적합한 클래스를 설명하시오.
- Core Data와 Sqlite 같은 데이터 베이스의 차이점을 설명하시오.
- JSON 데이터를 처리하는 방식과 파서, 객체 변환 방식에 대해 설명하시오.
- 웹 서버와 HTTP 연결을 사용해서 데이터를 주거나 받으려면 사용해야 하는 클래스와 동작을 설명하시오.
- Protocol에서는 왜 var만 되는지 설명하시요.
- DispatchQueue.main.sync를 사용하는 상황을 설명하시오.
- Run Loops에 대해 설명하시오.

## Objective-C
- Swift의 클로저와 Objective-C의 블록은 어떤 차이가 있는가?
- Mutable 객체과 Immutable 객체는 어떤것이 있는지 예를 들고, 차이점을 설명하시오.
- dynamic과 property 의미와 차이를 설명하시오.
- @property로 선언한 NSString* title 의 getter/setter 메서드를 구현해보시오.
- @property에서 atomic과 nonatomic 차이점을 설명하고, 어떤것이 안전한지, 어느것이 기본인지 설명하시오.
- @property로 선언한다는 것의 의미를 설명하고, .h에 넣을 경우와 .m에 넣을 경우 차이점을 설명하시오.
- -performSelector:withObject:afterDelay: 메시지를 보내면 인자값의 객체는 retain되는가? 그 이유를 함께 설명하시오.
- Objective-C 에서 캡슐화된 데이터를 접근하기 위한 방법들을 설명하시오.
- Fast Enumeration 이란 무엇인지 설명하시오. 
- unnamed category 방식에 대해 설명하시오.
- Category 확장과 Subclass 확장의 차이점을 설명하시오.
- Category 방식에 대해 설명하시오.
- Objective-C 에서 Protocol 이란 무엇인지 설명하시오.
- Objective-C++ 방식이 무엇인지 설명하고, 어떤 경우 사용해야 하는지 설명하시오.
</details>

### 예정

- [awesome-interview-question](https://github.com/DopplerHQ/awesome-interview-questions)
- [Tech-Interview by VSFe](https://github.com/VSFe/Tech-Interview)

-----------
## Algorithm
### 1주차
- [숫자 짝궁, 크레인 인형뽑기](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/milestone/1)
### 2주차
- [체육복](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/milestone/2)

------------
## Interview

### 1주차
- Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오. 
[Leeds](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/1%EC%A3%BC%EC%B0%A8/Delegate%20%ED%8C%A8%ED%84%B4%EC%9D%84%20%ED%99%9C%EC%9A%A9%ED%95%98%EB%8A%94%20%EA%B2%BD%EC%9A%B0%EB%A5%BC%20%EC%98%88%EB%A5%BC%20%EB%93%A4%EC%96%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Leeds), [PAKA](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/1%EC%A3%BC%EC%B0%A8/Delegate%20%ED%8C%A8%ED%84%B4%EC%9D%84%20%ED%99%9C%EC%9A%A9%ED%95%98%EB%8A%94%20%EA%B2%BD%EC%9A%B0%EB%A5%BC%20%EC%98%88%EB%A5%BC%20%EB%93%A4%EC%96%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/PAKA)
- Singleton 패턴을 활용하는 경우를 예를 들어 설명하시오. 
[Bible](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/1%EC%A3%BC%EC%B0%A8/Singleton%20%ED%8C%A8%ED%84%B4%EC%9D%84%20%ED%99%9C%EC%9A%A9%ED%95%98%EB%8A%94%20%EA%B2%BD%EC%9A%B0%EB%A5%BC%20%EC%98%88%EB%A5%BC%20%EB%93%A4%EC%96%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Bible), [Turing](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/1%EC%A3%BC%EC%B0%A8/Singleton%20%ED%8C%A8%ED%84%B4%EC%9D%84%20%ED%99%9C%EC%9A%A9%ED%95%98%EB%8A%94%20%EA%B2%BD%EC%9A%B0%EB%A5%BC%20%EC%98%88%EB%A5%BC%20%EB%93%A4%EC%96%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Turing)

### 2주차
- Optional 이란 무엇인지 설명하시오. 
[Leeds](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/2%EC%A3%BC%EC%B0%A8/Team/Optional%20%EC%9D%B4%EB%9E%80%20%EB%AC%B4%EC%97%87%EC%9D%B8%EC%A7%80%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Leeds), [Turing](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/2%EC%A3%BC%EC%B0%A8/Team/Optional%20%EC%9D%B4%EB%9E%80%20%EB%AC%B4%EC%97%87%EC%9D%B8%EC%A7%80%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Turing)
- Subscripts에 대해 설명하시오. 
[Bible](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/2%EC%A3%BC%EC%B0%A8/Team/Subscripts%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Bible), [PAKA](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/2%EC%A3%BC%EC%B0%A8/Team/Subscripts%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/PAKA)
- Class의 성능을 향상 시킬수 있는 방법들을 나열해보시오. 
[Bible](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/2%EC%A3%BC%EC%B0%A8/Personal/Class%EC%9D%98%20%EC%84%B1%EB%8A%A5%EC%9D%84%20%ED%96%A5%EC%83%81%20%EC%8B%9C%ED%82%AC%EC%88%98%20%EC%9E%88%EB%8A%94%20%EB%B0%A9%EB%B2%95%EB%93%A4%EC%9D%84%20%EB%82%98%EC%97%B4%ED%95%B4%EB%B3%B4%EC%8B%9C%EC%98%A4/Bible)
- Closure에 대하여 설명하시오. 
[Leeds](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/2%EC%A3%BC%EC%B0%A8/Personal/Closure%EC%97%90%20%EB%8C%80%ED%95%98%EC%97%AC%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Leeds)
- Protocol이란 무엇인지 설명하시오. 
[Turing](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/2%EC%A3%BC%EC%B0%A8/Personal/Protocol%EC%9D%B4%EB%9E%80%20%EB%AC%B4%EC%97%87%EC%9D%B8%EC%A7%80%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Turing)
- Struct가 무엇이고 어떻게 사용하는지 설명하시오. 
[PAKA](https://github.com/OpenBible3438/SwiftMaster17_Team13_CS_Study/tree/main/iOS/2%EC%A3%BC%EC%B0%A8/Personal/Struct%EA%B0%80%20%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0%20%EC%96%B4%EB%96%BB%EA%B2%8C%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EC%A7%80%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/PAKA)

### 3주차
- Codable에 대하여 설명하시오. 
[Bible](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/3%EC%A3%BC%EC%B0%A8/Codable%EC%97%90%20%EB%8C%80%ED%95%98%EC%97%AC%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Bible)
- Generic에 대해 설명하시오. 
[Leeds](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/3%EC%A3%BC%EC%B0%A8/Generic%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Leeds)
- instance 메서드와 class 메서드의 차이점을 설명하시오. 
[PAKA](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/3%EC%A3%BC%EC%B0%A8/instance%20%EB%A9%94%EC%84%9C%EB%93%9C%EC%99%80%20class%20%EB%A9%94%EC%84%9C%EB%93%9C%EC%9D%98%20%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%84%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/PAKA)
- struct와 class와 enum의 차이를 설명하시오. 
[Turing](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/3%EC%A3%BC%EC%B0%A8/struct%EC%99%80%20class%EC%99%80%20enum%EC%9D%98%20%EC%B0%A8%EC%9D%B4%EB%A5%BC%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Turing)

### 4주차
- Closure와 함수와의 관계에 대해 설명하시오.
[Turing](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/4%EC%A3%BC%EC%B0%A8/Closure%EC%99%80%20%ED%95%A8%EC%88%98%EC%99%80%EC%9D%98%20%EA%B4%80%EA%B3%84%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Turing)
- Extension 내부에서 함수를 override할 수 있는지 설명하시오.
[PAKA](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/4%EC%A3%BC%EC%B0%A8/Extension%20%EB%82%B4%EB%B6%80%EC%97%90%EC%84%9C%20%ED%95%A8%EC%88%98%EB%A5%BC%20override%ED%95%A0%20%EC%88%98%20%EC%9E%88%EB%8A%94%EC%A7%80%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/PAKA)
- Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.
[Leeds](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/4%EC%A3%BC%EC%B0%A8/Hashable%EC%9D%B4%20%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0%2C%20Equatable%EC%9D%84%20%EC%99%9C%20%EC%83%81%EC%86%8D%ED%95%B4%EC%95%BC%20%ED%95%98%EB%8A%94%EC%A7%80%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Leeds)
- MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.
[Bible](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/4%EC%A3%BC%EC%B0%A8/MVC%20%EA%B5%AC%EC%A1%B0%EC%97%90%20%EB%8C%80%ED%95%B4%20%EB%B8%94%EB%A1%9D%20%EA%B7%B8%EB%A6%BC%EC%9D%84%20%EA%B7%B8%EB%A6%AC%EA%B3%A0%2C%20%EA%B0%81%20%EC%97%AD%ED%95%A0%EA%B3%BC%20%ED%9D%90%EB%A6%84%EC%9D%84%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/Bible)

### 5주차
- 멀티쓰레드로 동작하는 앱을 작성하고 싶을 때 고려할 수 있는 방식들을 설명하세요.
[Leeds](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/5%EC%A3%BC%EC%B0%A8/%EB%A9%80%ED%8B%B0%EC%93%B0%EB%A0%88%EB%93%9C%EB%A1%9C%20%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%20%EC%95%B1%EC%9D%84%20%EC%9E%91%EC%84%B1%ED%95%98%EA%B3%A0%20%EC%8B%B6%EC%9D%84%20%EB%95%8C%20%EA%B3%A0%EB%A0%A4%ED%95%A0%20%EC%88%98%20%EC%9E%88%EB%8A%94%20%EB%B0%A9%EC%8B%9D%EB%93%A4%EC%9D%84%20%EC%84%A4%EB%AA%85/Leeds)
- 접근 제어자의 종류엔 어떤게 있는지 설명하시오.
[Turing](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/5%EC%A3%BC%EC%B0%A8/%EC%A0%91%EA%B7%BC%20%EC%A0%9C%EC%96%B4%EC%9E%90%EC%9D%98%20%EC%A2%85%EB%A5%98%EC%97%94%20%EC%96%B4%EB%96%A4%EA%B2%8C%20%EC%9E%88%EB%8A%94%EC%A7%80%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./Turing)
- Delegates와 Notification 방식의 차이점에 대해 설명하시오.
[PAKA](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/5%EC%A3%BC%EC%B0%A8/Delegates%EC%99%80%20Notification%20%EB%B0%A9%EC%8B%9D%EC%9D%98%20%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./PAKA)
- KVO 동작 방식에 대해 설명하시오.
[Bible](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/5%EC%A3%BC%EC%B0%A8/KVO%20%EB%8F%99%EC%9E%91%20%EB%B0%A9%EC%8B%9D%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./Bible)

### 6주차
- Copy On Write는 어떤 방식으로 동작하는지 설명하시오.
[PAKA](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/6%EC%A3%BC%EC%B0%A8/Copy%20On%20Write%EB%8A%94%20%EC%96%B4%EB%96%A4%20%EB%B0%A9%EC%8B%9D%EC%9C%BC%EB%A1%9C%20%EB%8F%99%EC%9E%91%ED%95%98%EB%8A%94%EC%A7%80%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./PAKA)
- Extension에 대해 설명하시오.
[Turing](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/6%EC%A3%BC%EC%B0%A8/Extension%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./Turing)
- property wrapper에 대해서 설명하시오.
[Leeds](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/6%EC%A3%BC%EC%B0%A8/property%20wrapper%EC%97%90%20%EB%8C%80%ED%95%B4%EC%84%9C%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./Leeds)
- Swift Standard Library의 map, filter, reduce, compactMap, flatMap에 대하여 설명하시오.
[Bible](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/6%EC%A3%BC%EC%B0%A8/Swift%20Standard%20Library%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./Bible)

### 7주차
- Result타입에 대해 설명하시오.
[Bible](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/7%EC%A3%BC%EC%B0%A8/Result%ED%83%80%EC%9E%85%EC%97%90%20%EB%8C%80%ED%95%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./Bible)
- String은 왜 subscript로 접근이 안되는지 설명하시오.
[Turing](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/7%EC%A3%BC%EC%B0%A8/String%EC%9D%80%20%EC%99%9C%20subscript%EB%A1%9C%20%EC%A0%91%EA%B7%BC%EC%9D%B4%20%EC%95%88%EB%90%98%EB%8A%94%EC%A7%80%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./Turing)
- TableView와 CollectionView의 차이점을 설명하시오.
[PAKA](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/7%EC%A3%BC%EC%B0%A8/TableView%EC%99%80%20CollectionView%EC%9D%98%20%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%84%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/PAKA)
- ViewController의 생명주기를 설명하시오.
[Leeds](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/7%EC%A3%BC%EC%B0%A8/ViewController%EC%9D%98%20%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0%EB%A5%BC%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4./Leeds)
