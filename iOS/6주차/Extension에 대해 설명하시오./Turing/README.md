# Extension에 대해 설명하시오.

확장(Extension)에 대해 알아보자 🙂

공식문서 

- 익스텐션을 이용해 클래스, 구조체, 열거형 혹은 프로토콜 타입에 기능을 추가할 수 있습니다. retroactive modeling으로 알려진 것과 같이 원본 코드를 몰라도 그 타입에 대한 기능을 확장할 수 있습니다.
- 익스텐션은 새 계산된 값을 추가할 수 있지만 새로운 저장된 프로퍼티나 프로퍼티 옵저버를 추가할 수는 없습니다.

?????… 공식문서 뭐래..?

**원본은 그대로 놔두고 (몰라도 됨) 내가 원하는 기능만 해당 타입에 추가**한다! 이 말인데

오호 코드로 보자구 !

```swift
extension Double {
    var km: Double { return self 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}
let oneInch = 25.4.mm
print("One inch is \(oneInch) meters")
// Prints "One inch is 0.0254 meters"
let threeFeet = 3.ft
print("Three feet is \(threeFeet) meters")
// Prints "Three feet is 0.914399970739201 meters"
```

**저장된 프로퍼티나 프로퍼티 옵저버를 추가할 수는** **없습니다**. 이 말은 뭐야?

```swift
extension Double {
		var km: Double = 0. // 오류 발생 Extensions must not contain stored properties
}
```

오.. **연산 프로퍼티만 가능**하다는 거군.. 

좀 더 가보자구 그럼 확장에 메서드를 추가해보자

함수 vs 메서드의 차이점은 알지? 우린 여기서 메서드만 볼거야

간략하게 **메서드는 클래스, 구조체, 열거형 속에 포함되어 있는 “함수”**를 지칭하는 말이지

그리고  메서드는 두 가지로 나뉘어

1. **인스턴스 메서드 (Instance Method)**
2. **타입 메서드 (Type Method)** - 형식(Type)에 연관된 메서드라서 인스턴스 생성 없이 Type이름만 알면 호출 오케이!

확장(extension) 레츠기!

```swift
// 1. 타입 메서드
extension Double {
    static func pi() {
        print(3.14)
    }
}

Double.pi()

// 2. 인스턴스 메서드
extension Double {
     func mulPi() {
         print(self * 3.14)
    }
}

let num = 2.0
num.mulPi()
```

그리구 좀 더 깊게 들어가보자 !

****변경 가능한 인스턴스 메소드 (Mutating Instance Methods)****

익스텐션에서 추가된 인스턴스 메소드는 인스턴스 자신(Self)를 변경가능한데 단!!! 구조체와 열거형 메소드 중 자기(Self)를 변경하려면 mutating 키워드 필수야!

```swift
extension Int {
    mutating func square() {
        self = self * self
    }
}
var someInt = 3
someInt.square()
// someInt is now 9
```

확장으로 ****서브스크립트 (Subscripts) 추가하기****

- 서브스크립트 - 클래스, 구조체, 열거형에서 시퀀스의 멤버 요소에 접근하기 위한 바로가기 첨자

즉 확장으로 내가 원하는 서브스크립트를 구현할 수 있다는 말이야!

```swift
extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
        for _ in 0..<digitIndex {
            decimalBase *= 10
        }
        return (self / decimalBase) % 10
        // 10 * n번째 수로 현재 수를 나눈 것의 나머지
      // 1인 경우 746381295 % 10 -> 5가 나머지
      // 2인 경우 746381295 % 10 -> 9가 나머지
    }
}
746381295[0]
// returns 5
746381295[1]
// returns 9
746381295[2]
// returns 2
746381295[8]
// returns 7
```

실제 프로젝트에서는 어디서 사용할까..? 

**확장으로 프로토콜 추가하기**

이건 소들이님 예제를 잠시 차용합니다. 

```swift
struct Human {
    let name: String
}
 
let sodeul: Human = .init(name: "sodeul")
let sodeulsodeul = sodeul
 
sodeul == sodeulsodeul      // error! Binary operator '==' cannot be applied to two 'Human' operands
```

보면 내가 선언한 Human 인스턴스끼리 비교인데 == 연산자가 에러가 뜸.. 이유는 **Equatable이라는 연산자를 채택을 해야 == 사용이 가능함!**

이렇게 하면 됩니까?

```swift
struct Human: Equatable {
    let name: String
 
    public static func == (lhs: Human, rhs: Human) -> Bool {
        return lhs.name == rhs.name
    }
}
```

굳굳.. 이렇게 해도 좋지만 실무에서는 아래처럼 하는걸 선언함 

```swift
struct Human {
    let name: String
}
 
extension Human: Equatable {
    public static func == (lhs: Human, rhs: Human) -> Bool {
        return lhs.name == rhs.name
    }
}
```

그러고보니 우리 테이블 뷰 만들때 커스텀하거나 확장으로 구현한거 생각나?

extension으로 UITableViewDataSource, UITableViewDelegate 추가해줬잖아 

UITableView 를 위한 Protocol 은 UITableViewDataSource, UITableViewDelegateProtocol는TableView를 이용하기 위해 해야할 일들의 목록, 즉 구현해야할 method인거지

요런식으로 쓰인다는 말씀 !!

```swift
// Data source object의 책임
// table 의 section 과 row 의 개수를 알려주는 것
// table 의 각 row 에 cell 을 제공하는 것
// section header 와 footer 에 title 을 제공하는 것
// table 의 index 를 설정하는 것
// 기본 데이터를 변경해야 하는 user 또는 table 초기 업데이트에 응답하는 것

extension ChatViewController: UITableViewDataSource{
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return messages.count
        
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "MessageCell", for: indexPath) as! MessageCell
        
        cell.label.text = messages[indexPath.row].body
        
        return cell
    }
    
    
}

// UITableViewDelegate의 책임
// custom header 와 footer view 를 만들고 관리하는 것
// row, header, footer 의 높이를 지정하는 것
// 더 나은 스크롤 지원을 위해 높이의 추정치를 제공하는 것
// row 의 들여쓰기 수준을 지정하는 것
// 선택된 row 에 대해 응답하는 것 -> 우린 이걸 주로 쓰지?
// table row 를 스와이프하는 등 액션에 응답하는 것
// table content 를 편집할 수 있도록 지원하는 것

extension ChatViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print(indexPath.row)
    }
    
}
```

끝!!
