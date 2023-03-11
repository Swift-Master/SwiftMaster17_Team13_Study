# URLSession에 대해서 설명하시오.

URLSession은 앱에서 서버와 데이터 통신을 하기 위한 HTTP 프로토콜 API를 제공하는 클래스이며 request, response 구조를 가지고 있습니다.

HTTP 통신을 위해서는 URLSession만이 유일한 방법은 아닌데요  
Alamofire와 Moya도 URLSession으로 구현된 서드 파티 라이브러리입니다. 

URLSession을 사용하려면,

1. Configuration 결정
2. Session 생성
3. Request url 설정
4. Task 결정 및 실행

순서로 동작됩니다.

### Configuration

Configuration은 `URLSessionConfiguration` 클래스의 객체로 URLSession을 생성하기 위해 필요한 요소이며 3가지 유형을 가지고 있습니다.

- .default : 기본 통신(쿠키와 같은 저장 객체 사용)
- .ephemeral : 쿠키나 캐시를 저장하지 않을 때 사용
- .background : 앱이 백그라운드 상태에 있을 때 데이터 다운로드/업로드 할 때 사용

### Session

네트워크 데이터 통신 작업들을 관리하는 URLSession 객체입니다.  
URLSession 클래스는 URL이 가리키는 리소스에 데이터를 다운로드하거나 업로드하는 API를 제공합니다.

**URLSession은 URLSessionConfiguration으로 생성한 Session을 이용해서 URLSessionTask를 생성하고 이 Task로 서버와의 통신을 시작하게 됩니다.**

### Task

Session 객체를 통해서 URLSessionTask의 객체  Task를 생성할 수 있습니다.

이 SessionTask의 종류는 4가지가 있는데,

1. Data Task
서버와 data를 NSData 형식으로 주고 받을 때 사용
2. Upload Task
Data Task와 비슷하지만 주로 file 같은 형태를 업로드 할 때 사용
3. Download Task
file 형태의 데이터를 받을 때 사용
4. Stream Task
RFC 6455에 정의된 WebSocket 프로토콜을 이용해서 양방향 TCP/IP 연결 메시지를 교환할 때 사용

주로 Data Task를 많이 이용하게 됩니다!

## 코드

글로만 설명을 하면 이해가 안 되니 코드로 봅시다.
iTunes Search API를 사용했습니다.

iTunes Search API는 https://itunes.apple.com/search?term=검색하려는문자열&media=movie 형식으로 통신해서 영화 정보를 얻어내려고 합니다.

### 순서 1. Configuration 결정

```swift
let config = URLSessionConfiguration.default // .default, .ephemeral, .background에서 상황에 맞는 것으로 설정
```

### 순서 2. Session 생성

```swift
let session = URLSession(configuration: config) // config를 사용하여 URLSession 객체 생성
```

### 순서 3. Request url 설정

```swift
var components = URLComponents(string: "https://itunes.apple.com/search?")

let term = URLQueryItem(name: "term", value: "spiderman")
let media = URLQueryItem(name: "media", value: "movie")

components?.queryItems = [term, media]
```

URLComponents는 URL을 구성하는 구조라고 설명되어 있는데 URL 값을 설정할 때 사용하고, URLQueryItem은 URL 뒤에 붙는 파라미터 값(=Query)을 세팅할 때 사용합니다.

URLQueryItem의 “term”의 value “spiderman”, “media”의 value “movie”를 세팅해서 components에 추가해줬기 때문에 components의 url 형태는 "https://itunes.apple.com/search?term=spiderman&media=movie"가 됩니다!

URL 설정은 URLComponents 대신 `URL` 객체를 사용해도 되긴 합니다.
ex) `let url = URL(string: "https://itunes.apple.com/search?")`

하지만 URL은 영문, 숫자, 특정 문자만 인식이 가능해서 한글이나 띄어쓰기를 제대로 처리해주지 못 합니다.  
URLComponents는 한글이나 띄어쓰기도 서버가 이해할 수 있도록 인코딩해주기 때문에 상황에 따라서 URL 혹은 URLComponents를 선택해주시면 될 것 같습니다.

```swift
print(components?.url)
// https://itunes.apple.com/search?term=spiderman&media=movie
```

url이 원하는대로 잘 설정된 것을 확인할 수 있습니다.

### 순서 4. Task 결정 및 실행

이제 Task를 통해서 서버에 url 요청을 해봅시다.

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

먼저 dataTask()를 살펴봅시다!

```swift
func dataTask(
    with url: URL,
    completionHandler: @escaping @Sendable (Data?, URLResponse?, Error?) -> Void
) -> URLSessionDataTask
```

파라미터로 넘긴 url의 작업이 끝나면 핸들러를 호출하게 됩니다.  
이 핸들러는 Data, URLResponse, Error 객체를 제공해주는데요!  
제가 작성한 코드에서는 각각 data, response, error의 변수명을 붙여서 사용하게 했습니다.  
이 변수명은 원하는대로 바꿔서 사용해도 됩니다.

data, response, error에는 url 호출의 결과값이 담겨있습니다.

response에는 호출 후 상태코드를 확인할 수 있는 statusCode(200은 정상처리 코드, 400이나 500번대는 에러코드)를 제공합니다.

error에는 호출 후 생긴 에러 내용을 담고,  
data에는 호출 후 받은 데이터들을 담습니다.

data는 Data 타입으로 넘어오기 때문에 인코딩하는 과정이 필요합니다.

그리고 통신을 실행한다는 `task.resume()` 코드를 꼭 작성해주어야 됩니다!

### 전체 코드

```swift
func requestMovie() {
    // 순서 1 Configuration 결정
    let config = URLSessionConfiguration.default // .default, .ephemeral, .background에서 상황에 맞는 것으로 설정

    // 순서 2 Session 생성
    let session = URLSession(configuration: config) // config를 사용하여 URLSession 객체 생성

    // 순서 3 Request url 설정
    var components = URLComponents(string: "https://itunes.apple.com/search?")

    let term = URLQueryItem(name: "term", value: "spiderman")
    let media = URLQueryItem(name: "media", value: "movie")

    components?.queryItems = [term, media]

    //print(components?.url)

		// 순서 4 Task 결정 및 실행
    guard let requestUrl = components?.url else { return } // 값을 가져오지 못 하면 함수 종료
    let task = session.dataTask(with: requestUrl) { (data, response, error) in
        
        print( (response as! HTTPURLResponse).statusCode ) // 상태코드 출력
        
        guard error == nil else { return } // error 존재하면 함수 종료
        
        if let responseData = data {
            let dataString = String(data: responseData, encoding: .utf8) // Data 타입 -> String 변환
            print(dataString!) // 데이터 출력
        }
    }
    
    // Task 통신 실행
    task.resume()
}

// 함수 실행
requestMovie()
```

### 결과

<img width="1244" alt="Untitled" src="https://user-images.githubusercontent.com/59015538/224049762-7e9609df-5e3b-45e9-9e51-4f10abd62e8f.png">

콘솔창에 정상처리를 나타내는 200과 Data를 String으로 변환한 JSON 형식의 문자열이 출력됐습니다.

이게 정상적인 데이터인지 제대로 확인해봅시다.

<img width="1087" alt="스크린샷 2023-03-09 오후 10 50 12" src="https://user-images.githubusercontent.com/59015538/224050007-56331825-bfda-4029-86ff-3905bf38b59a.png">

POSTMAN으로 예상 결과값을 확인해보니 resultCount:12에 통신이 정상적으로 된 것 같습니다!

## 참고

[URLSession과 Alamofire](https://velog.io/@zaezero/URL-Session%EA%B3%BC-Alamofire)

URLSession과 URLSession 기반으로 만들어진 Alamofire의 차이점을 다룬 글입니다.
URLSession은 꽤나 코드가 복잡한데 Alamofire는 훨씬 간결하게 HTTP 통신을 할 수 있습니다.
(현업에서도 URLSession은 잘 사용하지 않는다고 하네요!)
