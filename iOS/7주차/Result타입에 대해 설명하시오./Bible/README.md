# Result타입에 대해 설명하시오.

```swift
@frozen enum Result<Success, Failure> where Failure : Error
```

Result 타입은 경우에 따른 관련된 값을 포함하여 성공 또는 실패를 나타내는 값이라고 설명하고, enum으로 작성되어 있습니다.

앱 실행 중에 에러가 발생하면 프로그램은 강제로 종료됩니다.  
(컴파일 에러는 문법과 관련있기 때문에 앱 실행 전에 에러가 발생하고, 런타임 에러는 앱 실행 중에 발생한 에러로 강제 종료 상황이 올 수 있음)

하지만 발생 가능한 에러에 대한 처리를 해주면 강제 종료 없이 앱을 계속 실행시킬 수 있어서 개발자는 에러 처리를 위한 코드를 작성해야 합니다.

Swift 1.x 최초 버전에서는 Objective-C와 동일한 방법으로 오류 처리를 했습니다. NSError 포인터를 사용했기 때문에 포인터 사용을 지양하는 Swift에서는 적절하지 않은 방법이었습니다.

Swift 2.x 부터는 새로운 에러처리 방식이 도입되어 지금까지 사용하고 있습니다. do, try, catch, throw를 통해서 발생한 에러를 처리합니다.

Result 타입은 Swift 5에서 도입된 새로운 에러 처리 방법입니다. 

## 일반적인 에러 처리 방법

주민등록번호를 입력할 때 생길 수 있는 에러들에 대해서 예를 들어보겠습니다.

1. 열거형 선언  
에러 발생 가능한 상황을 열거형으로 작성해둡니다.  
이 열거형은 `Error` 프로토콜을 채택합니다.

```swift
enum IdNumberError: Error {
    case overLength     // 길이가 길 때
    case shortLength    // 길이가 짧을 때
    case notNumber      // 숫자가 아닐 때
}
```

주민등록번호 앞 6자리와 뒤 7자리 총 13자리의 숫자만 입력받는다고 할 때  
길이가 13자리를 넘을 때, 길이가 13자리보다 적을 때, 숫자가 아닌 문자가 들어왔을 때를 에러 처리하기로 하고 Error 프로토콜을 채택한 enum 타입으로 에러를 정의했습니다.

2. throwing 함수 정의

```swift
func validateId(id: String) throws -> Bool {
    if id.count > 13 {
        throw IdNumberError.overLength
    } else if id.count < 13 {
        throw IdNumberError.shortLength
    }
    
    if Int(id) != nil {
        return true
    } else {
        throw IdNumberError.notNumber
    }
}
```

에러가 발생하려면 어떠한 행위를 해야되니까 그 행위를 할 함수를 정의합니다.  
이 함수는 에러가 발생하면 에러를 던지겠다는 의미로 `throws` 키워드를 함께 작성해주어야 합니다.

그리고 에러가 발생하는 지점에서는 이 에러가 어떤 에러인지 enum으로 작성해두었던 것을 `throw` 키워드를 사용하여 에러를 던져줍니다.

3. 함수 실행

```swift
do {
    try validateId(id: "230303-333333")
    print("주민등록번호가 정상적으로 입력됐습니다.")
} catch IdNumberError.overLength {
    print("주민등록번호 입력값 길이 초과 에러")
} catch IdNumberError.shortLength {
    print("주민등록번호 입력값 길이 부족 에러")
} catch IdNumberError.notNumber {
    print("숫자 이외의 문자 에러")
}
```

`do-catch` 구문을 사용해서 함수를 실행하고 에러 처리하는 코드를 작성할 수 있습니다.  
`do` 안에는 에러를 발생할 수 있는 함수(throwing 함수)를 실행하는데 이 함수는 `try` 키워드를 사용해서 실행해야합니다.  
`catch` 구문에서는 함수 실행으로 발생하는 각 에러에 대한 처리 로직을 작성할 수 있습니다.

## Result 타입을 사용한 에러 처리 방법

위에서와 똑같은 예제를 Result 타입으로 변형해봅시다!

1. 열거형 선언

```swift
enum IdNumberError: Error {
    case overLength     // 길이가 길 때
    case shortLength    // 길이가 짧을 때
    case notNumber      // 숫자가 아닐 때
}
```

에러를 정의하는 열거형을 만드는 것은 동일하게 작성해야 합니다.

2. Result 타입 함수

```swift
func validateId(id: String) -> Result<Bool, IdNumberError> {
    if id.count > 13 {
        return .failure(.overLength)
    } else if id.count < 13 {
        return .failure(.shortLength)
    }
    
    if Int(id) != nil {
        return .success(true)
    } else {
        return .failure(.notNumber)
    }
}
```

이 함수의 리턴 타입을 Result 타입으로 바꾸어 다시 작성했습니다.

맨 위에서 Result 타입이 어떻게 작성되어있는지 다시 한 번 봐보면

```swift
@frozen enum Result<Success, Failure> where Failure : Error
```

제네릭으로 Success, Failure를 받는데요

Success에는 에러가 발생하지 않고 정상 처리되었을 때의 타입을 작성해주면 되고,  
Failure에는 에러가 발생했을 때의 타입을 작성해주면 됩니다.

주민등록번호가 제대로 들어오면 Bool 타입 true를 사용하기 때문에 Success 자리에는 Bool을 작성했고,  
에러가 발생하게 되면 IdNumberError라는 열거형에서 어떤 에러인지 판별해주니까 IdNumberError을 작성하면 됩니다.

또한 Result는 열거형이기 때문에 함수를 실행하면서 `return .failure(.overLength)`와 `return .success(true)` 처럼 실패(에러), 성공 케이스를 나누어서 작성하게 됩니다.

위에서 `trow`로 에러를 던져주는 것이 아닌 `return`을 사용해서  어떤 에러인지 `반환값`으로 보내주게 됩니다.

3. 함수 실행

```swift
let isValidatedIdNumber = validateId(id: "230303-333333")
switch isValidatedIdNumber {
case .success(true):
    print("주민등록번호가 정상적으로 입력됐습니다.")
case .success(false):
    print("알 수 없는 에러")
case .failure(.overLength):
    print("주민등록번호 입력값 길이 초과 에러")
case .failure(.shortLength):
    print("주민등록번호 입력값 길이 부족 에러")
case .failure(.notNumber):
    print("숫자 이외의 문자 에러")
}
```

Result 타입으로 작성된 함수는 처리 결과값을 return하기 때문에 이 값을 바인딩 할 수 있습니다.  
바인딩된 값은 `switch`문을 이용해서 결과값으로 받을 수 있는 케이스 별로 로직을 구현할 수 있습니다.

## 두 방법의 차이

일반적인 방법으로 에러 처리를 하게 되면 `throws`, `throw`, `do-catch`, `try` 여러 키워드들을 사용해서 로직을 작성하지만 Result<>를 사용하면 이러한 키워드들을 사용하지 않고 비교적 깔끔한 코드를 작성할 수 있습니다.

Result<> 방법이 더 좋기 때문에 도입된 것이 아닌 두 방법의 가독성이나 작성 방식의 차이가 있기 때문에 모두 사용할 수 있으니 개발자의 선택의 폭을 넓혀주는 것으로 보면 될 것 같습니다.
