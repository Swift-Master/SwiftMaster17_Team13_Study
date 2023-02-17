# KVO 동작 방식에 대해 설명하시오.

KVO는 Key-Value Observing으로 특정 키 값의 변화를 감지하고 알려주는 코코아 프로그래밍 패턴입니다. 

이 기능은 모델과 뷰처럼 앱의 논리적으로 분리되어 있는 사이 간 변경 사항을 전달하는데 유용하게 쓰입니다. → 모델과 뷰는 서로 독립적으로 작동하는 건 지난 주 MVC 패턴에 대해 다뤘을 때의 글을 생각해보시면 됩니다!

이 기능은 `NSObject`에서 상속받은 클래스에서만 키 값 관찰을 할 수 있기 때문에 Swift에서 사용하려면 `@objc` 어노테이션을 붙여서 사용해야합니다.

```swift
class MyObjectToObserve: NSObject {
    @objc dynamic var myDate = NSDate(timeIntervalSince1970: 0) // 1970
    func updateDate() {
        myDate = myDate.addingTimeInterval(Double(2 << 30)) // Adds about 68 years.
    }
}
```

공식 문서에서 제공하는 이 코드를 먼저 분석해보면,

Swift 런타임과 Objective-C 런타임은 다르게 동작하게 되는데 `dynamic` 키워드를 통해서 Objective-C 런타임을 사용한 동적 디스패치를 가능하게 하고, objc 속성이 암시적으로 표시되게 합니다.

`NSDate` 객체는 Swift에서 제공하는 Date와 동일한 기능을 하는 객체로 보면 되는데, 차이점이 있다면 NSDate는 NSObjcet를 상속받은 클래스이고, Date는 구조체로 작성되어 있습니다.

`NSDate(timeIntervalSince1970: TimeInterval)`은 파라미터에 초(sec)를 넣으면 1970년 1월 1일 00:00:00 UTC 기준으로 시간을 계산해서 반환해줍니다.

```swift
var testMyDate = NSDate(timeIntervalSince1970: 60)
print(testMyDate) // 1970-01-01 00:01:00 +0000
```

60초를 넣었으면 이렇게!

updateDate()에 쓰인 `addingTimeInterval(TimeInterval)`은 기존에 계산된 값에다가 추가적으로 초 단위를 더해서 계산해주는 함수입니다.

```swift
var testMyDate = NSDate(timeIntervalSince1970: 60)
print(testMyDate) // 1970-01-01 00:01:00 +0000

print(testMyDate.addingTimeInterval(62)) // 1970-01-01 00:02:02 +0000
```

60초였던 testMyDate에 62초를 추가해서 2분 2초를 반환합니다.

그런데, 공식 문서에서 제공하는 코드에는 `Double(2 << 30)` 이런 걸 사용했는데요!  
이건 비트 시프트 연산자로, 01010101 이런 정수 데이터의 비트를 이동 시켜서 데이터 값을 변화시키는 건데 여기서 이 내용은 중요하지 않으니 코드를 훨씬 쉽게 바꿔서 봅시다.

```swift
class MyObjectToObserve: NSObject {
    @objc dynamic var myDate = NSDate(timeIntervalSince1970: 60)
    func updateDate() {
        myDate = myDate.addingTimeInterval(62)
    }
}
```

이제 어떤 코드인지 읽혀지죠!

NSObject 클래스를 상속한 MyObjectToObserve 클래스의 @objc, dynamic 키워드를 붙인 myDate는 이제 KVO 방식으로 값의 변화를 우리가 감시할 수 있게된 것을 알 수 있습니다.

```swift
class MyObserver: NSObject {
    @objc var objectToObserve: MyObjectToObserve
    var observation: NSKeyValueObservation?

    init(object: MyObjectToObserve) {
        objectToObserve = object
        super.init()

        observation = observe( \.objectToObserve.myDate, options: [.old, .new] ) { object, change in
            print("변경 전 값: \(change.oldValue!), 변경 후 값: \(change.newValue!)")
        }
    }
}
```

이제 실제로 값을 감시할 MyObserver 클래스를 만들고, `NSKeyValueObservation` 인스턴스의 `observe(_ keyPath:options:changeHandler:)`를 통해서 감시할 수 있습니다.

`\.objectToObserve.myDate`는 MyObjectToObserve의 myDate 키 경로를 나타냅니다. 

또한 값의 변화를 알지 않아도 되면 options: 를 생략해도 됩니다. 
그럼 .oldValue와 .newValue의 값은 nil이 됩니다.

```swift
let myObject = MyObjectToObserve()
let myObserver = MyObserver(object: myObject)

myObject.updateDate()
// 콘솔 print
// 변경 전 값: 1970-01-01 00:01:00 +0000, 변경 후 값: 1970-01-01 00:02:02 +0000
```

감시하는 myObserver에 감시 당할 myObject를 전달하여 둘을 연결해주었습니다.

그리고 감시 당하는 myObject의 myDate 속성의 값이 변경될 수 있도록 updateDate()를 실행시키면 감시 당한 결과값이 아주 잘 출력되는 것을 볼 수 있습니다!

## willSet, didSet 차이

거의 동일한 기능인 값의 변화를 감지하는 willSet과 didSet과의 차이에 대해 궁금할 수 있는데요,

우리가 타입을 직접 만드는 경우에는 willSet, didSet을 구현해서 값의 변화를 알 수 있겠지만, 다른 사람이나 외부 라이브러리에서 정의한 타입이면 내부 소스를 마음대로 변경해서 사용할 수 없을테니 이럴 때는 KVO 방식으로 값의 변화를 관찰할 수 있다네요!

### 출처

- [https://developer.apple.com/documentation/swift/using-key-value-observing-in-swift](https://developer.apple.com/documentation/swift/using-key-value-observing-in-swift)
- [https://yagom.net/forums/topic/kvo와-willset-didset/](https://yagom.net/forums/topic/kvo%EC%99%80-willset-didset/)
