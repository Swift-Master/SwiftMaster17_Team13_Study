# Codable에 대하여 설명하시오.

앱을 만들 때 서버와 통신을 해서 데이터를 주고 받는 경우가 많은데 특히 요즘에는 JSON 형식으로 된 데이터를 주고 받는 경우가 흔합니다.

애플 공식 문서에서 Codable은 “A type that can convert itself into and out of an external representation.”

자신을 변환하거나 외부 표현으로 변환하는 타입이라고 말하는데, 여기서 외부 표현이라는 것은 JSON 형식과 같은 데이터 포맷을 말하고, 변환이라는 것은 Encode하거나 Decode하는 것으로 이해하면 됩니다.

```swift
typealias Codable = Decodable & Encodable
```

실제로 Codable은 Decodable과 Encodable이라는 두 프로토콜을 합쳐놓은 형태이므로 class, struct, enum에 Codable을 채택해서 사용하면 됩니다.
(보통 데이터를 다루는 Model을 Swift에서는 struct에 주로 작성합니다 !)

### 예제 - Encode

회원가입을 할 때 email, id, nickname을 담아서 서버에 JSON 형태로 보내야한다고 가정해보겠습니다.

```swift
struct UserModel: Codable {
    var email: String
    var id: String
    var nickname: String
}
```

UserModel이라는 구조체에 Codable 프로토콜을 채택하여 보낼 데이터들을 작성했습니다.

```swift
let join = UserModel(email: "zzagsk@gmail.com", id: "zzagsk", nickname: "Bible")

do {
    let dataEncode = try? JSONEncoder().encode(join) 
    print(dataEncode)
    // Optional(61 bytes)
    // 변환된 Data형식을 그대로 출력했기 때문에 bytes 크기를 출력
    
    if let dataString = String(data: dataEncode!, encoding: .utf8) {
        print(dataString)
	// {"email":"zzagsk@gmail.com","id":"zzagsk","nickname":"Bible"}
	// 변환된 Data 형식을 String으로 형변환하여 출력
    }
}
```

join 변수에는 UserModel의 값을 지정해서 담아주고, 이 데이터들을 JSON 형태로 Encode 시켜주었습니다.

`JSONEncoder().encode()`에는 Encodable한 값을 넣어 JSON 형태로 **Encode** 할 수 있습니다.
Codable을 채택한 UserModel은 Encodable의 성격도 가지고 있기 때문에 join을 변환할 수 있습니다.

이렇게 변환한 값은 서버에 보낼 수 있고, 또한 서버에서는 이렇게 JSON 형태의 값을 보내주어 받게 되죠.

### 예제 - Decode

하지만 위의 코드에서 print한 dataEncode 변수 같은 byte크기나, String으로 형변환한 dataString 변수 같은 문자열은 우리가 사용하기에는 많이 불편하기 때문에 데이터를 개발자가 사용하기 편하도록 **Decode**하는 과정이 필요합니다.

위의 예제에서 Encoding한 데이터를 서버에서 받아온 데이터로 가정해봅시다.

```swift
let join = UserModel(email: "zzagsk@gmail.com", id: "zzagsk", nickname: "Bible")

var dataDecode: UserModel!
do {
    // 서버에서 받아온 데이터 {"email":"zzagsk@gmail.com","id":"zzagsk","nickname":"Bible"}
    let dataEncode = try JSONEncoder().encode(join)
    
    dataDecode = try JSONDecoder().decode(UserModel.self, from: dataEncode)
}

print(dataDecode.email) // zzagsk@gmail.com
print(dataDecode.id) // zzagsk
print(dataDecode.nickname) // Bible
```

UserModel 타입을 가지는 변수 dataDecode는 Data(JSON)형식으로 Encoding된 dataEncode 변수를 UserModel 형식으로 Decoding한 값을 저장합니다.

`JSONDecoder().decode()`의 첫 번째 파라미터에는 Decodable한 값이 들어가야 하므로 Decodable한 성격도 가진 Codable을 채택한 UserModel이 들어올 수 있게 됩니다.

이렇게 JSON Data 형식을 구조체(UserModel)에 담아서 값을 꺼내서 쓸 수 있게 Decode 했습니다.

### 결론

- Encodable : 객체를 Data 형식으로 변환
- Decodable: Data 형식을 객체로 변환

Codable은 Encodable 프로토콜과 Decodable 프로토콜의 속성을 모두 갖는 타입으로 객체를 Data로, Data를 객체로 변환해야 할 때 채택하여 사용하는 프로토콜로 정리할 수 있습니다 !
