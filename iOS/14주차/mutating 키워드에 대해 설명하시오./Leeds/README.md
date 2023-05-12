# mutating 키워드에 대해 설명하시오.

### mutating ?

- 값 타입(struct, enum)의 속성을 수정할 때 사용하는 키워드
- 해당 메서드가 호출되면 실제 복사를 해야한다고 알려주는 역할

<br>

`mutating` 을 선언한 메서드는 메서드 내에서 프로퍼티를 변경할 수 있고, 메서드가 종료될 때 변경한 모든 내용을 원래 struct에 다시 기록한다.

<br>

```swift
// mutating 키워드 관련 예제 (내부에서 값 변경)

struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}

var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)

print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)"
```

```swift
// swift 공식문서 mutating 키워드 관련 예제 (새로운 인스턴스 할당)

struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}

var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0) // → somePoint 의 값 : (3.0, 4.0)

print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)"
```

<br>


---

[Methods - The Swift Programming Language (Swift 5.5)](https://docs.swift.org/swift-book/LanguageGuide/Methods.html)
