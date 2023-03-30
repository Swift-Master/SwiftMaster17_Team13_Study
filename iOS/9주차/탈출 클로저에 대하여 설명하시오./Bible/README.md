# 탈출 클로저에 대하여 설명하시오.

일반적인 클로저(Non-Escaping Closure)는 어떤 함수의 파라미터로 쓰이게 되면 함수 내부에서 동작을 하게 됩니다. 

```swift
// Non-Escaping Closure
var printHelloClosure = {
    print("Hello!")
}

func runNonEscapingClosure(closure: () -> Void) {
    closure()
    print("함수 종료")
}

runNonEscapingClosure(closure: printHelloClosure)
/*
	Hello!
	함수 종료
*/
```

이런 느낌이지요.

탈출 클로저(Escaping Closure)는 함수의 파라미터에 작성하고 실행은 함수가 다 끝나고 결과값이 반환될 때 작동하게 됩니다. 함수의 작업이 완료되기 전까지 클로저는 호출되지 않습니다.

위의 예시에서 다룬 Non-Escaping Closure는 함수가 종료되면 파라미터로 들어간 클로저의 기능도 사라지게 되지만, 탈출 클로저는 함수가 종료될 때 사라지지 않고 이 함수에서 탈출해서 실행을 하겠다! 라고 이해하면 될 것 같습니다.

함수의 파라미터는 해당 함수의 scope 안에서만 유효했던 것인데 밖으로 탈출해서 함수의 scope 밖에서 사용한다는 것은 함수 밖의 변수에 담아놓을 수 있다는 뜻이기도 합니다. 

```swift
// 클로저를 담을 변수
var closureVariable: () -> () = {}

var printHelloClosure = {
    print("클로저 실행")
    print("Hello!")
}

func runNonEscapingClosure(closure: () -> Void) {
    closureVariable = closure // error!!
    print("함수 종료")
}
```

외부에 클로저를 담을 변수 closureVariable를 선언하고,  
함수 안에 쓰였던 클로저를 담게 되면 에러가 발생하게 됩니다.

에러의 내용은 `“Assigning non-escaping parameter 'closure' to an @escaping closure”` 탈출 클로저가 아닌 것에 할당했기 때문에 에러가 난다고 알려줍니다.

일반적인 클로저는 함수 내부의 scope에서만 작동할 수 있는데 scope를 벗어나려는 행위니까 에러가 나게 되는 거죠. 탈출 클로저로 선언하게 되면 에러가 나지 않고 scope를 벗어나는 것이 가능하게 됩니다.

```swift
var closureVariable: () -> () = {}

var printHelloClosure = {
    print("클로저 실행")
    print("Hello!")
}

func runNonEscapingClosure(closure: @escaping () -> Void) { // @escaping으로 에러 해결
    closureVariable = closure
    print("함수 종료")
}

runNonEscapingClosure(closure: printHelloClosure)

closureVariable() // 클로저 실행\n Hello!\n
```

탈출 클로저는 **`GCD 비동기`** 처리 방식으로 작성할 때와 GCD가 아닌 **`completionHandler`** 로 사용되면  `@escaping` 키워드를 붙여서 사용하면 됩니다.

completionHandler ⇒ 완료 핸들러 이름 그대로 비동기적인 작업이 완료되었을 때 실행되는 클로저를 말합니다. 콜백 함수를 생각하면 됩니다.

사용방식을 보니 탈출 클로저는 비동기 처리가 필요할 때 사용한다는 것을 알 수 있습니다.

```swift
func dataTask(
    with url: URL,
    completionHandler: @escaping @Sendable (Data?, URLResponse?, Error?) -> Void
) -> URLSessionDataTask
```

제가 지난주에 URLSession을 다루면서 dataTask()를 설명했었는데요!  
이 dataTask()에도 `@escaping` 키워드가 붙은 걸 볼 수 있습니다.  
dataTask()의 어떤 처리가 끝난 뒤에 탈출 클로저로 비동기 처리를 한다는 의미인 것이죠.

```swift
guard let requestUrl = components?.url else { return } // url이 정상적으로 만들어지지 않으면 함수 종료
let task = session.dataTask(with: requestUrl) { (data, response, error) in
    
    print( (response as! HTTPURLResponse).statusCode ) // 상태코드 출력 (200 정상)
    
    guard error == nil else { return } // error 존재하면 함수 종료
    
    if let responseData = data {
        let dataString = String(data: responseData, encoding: .utf8) // Data 타입 -> String 변환
        print(dataString!) // 데이터 출력
    }
}

// Task 통신 실행
task.resume()
```

지난주에 사용했던 코드를 그대로 가져왔습니다.

dataTask()가 서버와의 통신을 모두 마치는 일을 진행한 뒤에 탈출 클로저를 사용해서 data, response, error 결과값을 볼 수 있도록 비동기 처리를 해주는 것이죠!

만약 비동기 처리를 하지 않는다면 서버에서 데이터를 받아오는 그 시간동안 화면 전환이 되어서 화면에 제대로 데이터를 못 보여주는 일이 생길 수 있겠죠.

이런 서버 통신 이외에도 비동기 처리가 필요한 경우나 함수 종료 직후의 어떤 처리를 하기 위해서 탈출 클로저를 사용하면 될 것 같습니다👍
