# 스위프트 가이드북
[https://docs.swift.org/swift-book/LanguageGuide/Subscripts.html](https://docs.swift.org/swift-book/LanguageGuide/Subscripts.html)
- 정의 ?
    - 클래스, 구조체, 열거형에서 컬렉션, 리스트, 시퀀스(배열,set,딕셔너리)의 멤버 요소에 접근하는 축약 문법!
    - 인스턴스이름 뒤에 대괄호( [index ] )의 형태. 내부 구현 형태는 계산 속성과 비슷하다.
    
    ```swift
    subscript(index: Int) -> Int { // 와일드카드(_)가 없는데도 호출 시 아규먼트 생략가능
        get {
            // 읽기만 가능하게 구현가능
        }
        set(newValue) {
            // 세부 구현. newValue를 파라미터로 주지 않아도 기본적으로 사용가능.
        }
    }
    ```
    
- ex) 딕셔너리에서의 서브스크립트 사용

```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2 
// 키값이 없을수도 있는 경우도 있기 때문에 서브스크립트의 반환형은 옵셔널 타입이다.
```

- 함수와 유사하게 가변 매개변수, 기본값 등을 매개변수로 사용가능하나 in-out 매개변수는 불가능

```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double { // 두 개의 파라미터도 가능
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}

matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
```

- 타입 메서드처럼 타입에서만 사용가능한 타입 스크립트도 있음.

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
    static subscript(n: Int) -> Planet { //클래스는 class 키워드
        return Planet(rawValue: n)!
    }
}
let mars = Planet[4]
print(mars)
```
</br>

# 애플 공식문서
[https://developer.apple.com/documentation/swift/collection/subscript(_:)-887tm](https://developer.apple.com/documentation/swift/collection/subscript(_:)-887tm)
## 대부분의 컬렉션

- 기본 구현이 제공되는 required 메서드다

```swift
subscript(Range<Self.Index>) -> Slice<Self>
subscript<R>(R) -> Self.SubSequence
subscript((UnboundedRange_) -> ()) -> Self.SubSequence
```

- 기본 구현 (subscript(Range<Self.Index>) -> Slice<Self>)

```swift
let streets = ["Adams", "Bryant", "Channing", "Douglas", "Evarts"]
let streetsSlice = streets[2...]
print(streetsSlice)
// ["Channing", "Douglas", "Evarts"]

//반환된 Slice는 이전 컬렉션의 같은 요소에 같은 인덱스로 접근한다.
let index = streetsSlice.firstIndex(of: "Evarts")    // 4
print(streets[index!])
// "Evarts"

//즉 기존 컬렉션의 부분을 사용하기 때문에, 새 배열로 착각하면 안된다.

print(streetsSlice.startIndex)
// 2
print(streetsSlice[2])
// "Channing"

print(streetsSlice[0])
// error: Index out of bounds
```

- 컬렉션에서 사용시 서브스크립트의 파라미터 인덱스는 0부터 시작한다. 따라서 마지막 인덱스는 순서와 같지 않다.

```swift
var streets = ["Adams", "Bryant", "Channing", "Douglas", "Evarts"]
print(streets[1])
// Prints "Bryant", Bryant는 두번쨰 요소지만 인덱스는 1이다.
```

- 시간복잡도 O(1)

## 딕셔너리

- `subscript(Key, default _: () -> Value) -> Value`
    - 닐 코얼레싱과 유사한 방식, 해당 key가 딕셔너리에 없다면 nil을 반환하는 대신 기본값을 반환

```swift
let message = "Hello, Elle!"
var letterCounts: [Character: Int] = [:]
for letter in message {
    letterCounts[letter, default: 0] += 1
}
// letterCounts == ["H": 1, "e": 2, "l": 4, "o": 1, ...]

//dictionary의 value 타입이 클래스라면 서브스크립트를 이용해 value를 수정하면 안된다.
//dictionary에 작업 후 기본값과 키가 다시 기록되지 않기 때문이다?
```

UIKit에서 사용되는 서브스크립트

- UIViewConfigurationState, UICellConfigurationState, UIConfigurationState
    - 사용자가 커스터마이징 설정을 할때 사용하는 구조체들이다.

```swift
subscript(key: UIConfigurationStateCustomKey) -> AnyHashable? { get set }
//딕셔너리, Set처럼 value(Custom State)를 hashable하게 저장한다.
```
