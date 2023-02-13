# instance 메서드와 class 메서드의 차이점을 설명하시오.

## 메서드란?
### 정의
- 인스턴스(구조체, 클래스) 내부에 구현된 함수로, 원하는 동작을 정의할 수 있다.
- 메모리 공간이 할당되어 있지 않다.(함수의 특성, 필요한 시점에 스택에 호출되었다 실행후 소멸)

### 종류
- 생성자, 서브스크립트, 인스턴스 메서드, 타입 메서드. 
- 사실상 메서드이나 속성인 계산 속성도 존재

### 

## instance 메서드?

### 정의
```
class Example {
  var ex1 : Int = -1
  var ex2 : Int = 1
  func instanceMethod() {
  print("\(ex1) + \(ex2) = \(ex1+ex2)")
  }
}
```
- 위치만 클래스나 구조체 내부에 있을 뿐, 형태는 함수와 동일

### 특징

- 메서드에 접근하려면 인스턴스 명으로 접근해야 한다.


```
var instance1 = Example()
instance1.instanceMethod() // 0
```
- 클래스 제외 mutating 키워드 없이는 내부 속성을 변경할 수 없다. 또, 인스턴스가 상수로 선언되었다면, mutating 키워드가 있더라도 내부 속성을 변경할 수 없다.

```
struct cons {
  var attribute1 : String?
  var attribute2 : Int?

  mutating func changeAttribute2(variable : Int?) {
      self.attribute2  = variable  
  }
}

var changed = cons()
changed.changeAttribute2(variable : 3) // attribute2 = 3
let immutable = cons()
changed.changeAttribute2(variable : 4) // 컴파일러가 에러를 표시함.
```

## 타입(static, class) 메서드는?

### 특징
- 생성된 인스턴스가 아닌 타입에 귀속된 메서드로서 사용시 해당 타입명으로 호출한다.
`Int.random(in:1...100) `
- 인스턴스 메서드와 구분하기 위해 static 키워드를 붙힌다.
- class 키워드는 해당 타입 메서드를 재정의(override)해서 사용하고 싶을 때 붙힌다. (보통의 언어에서 static 메서드는 재정의 불가능하다)

```
class Master {
    
    static func basic() {
        print("ABCDEFG")
    }
  class func order() {
    print("물")
  }
}

class Slave: Master {
    
/*    override static func basic() {
         print("외안되")
     } 
타입 메서드는 class키워드 없이는 재정의 불가능하다. */
    
    override class func order() {
        print("내가왜")
    }
}
Master.basic() //ABCDEFG
Master.order() // 물
Slave.order() // 내가왜
Slave.basic() // ABCDEFG -> 재정의가 불가능할 뿐, 상속은 문제없이 가능하다.
```

## 참고 자료 및  읽을거리

### [인스턴스 메서드와 타입 메서드의 차이? (스택오버플로우)](https://stackoverflow.com/questions/28551953/the-difference-between-swifts-instance-methods-and-type-methods)

### [타입 메서드는 언제 사용할까](https://sujinnaljin.medium.com/swift-static%EA%B3%BC-class-method-property-%ED%9A%A8%EA%B3%BC%EC%A0%81%EC%9C%BC%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-b336311a923c)
### [swift 공식 가이드북 한글 번역 - 메서드](https://xho95.github.io/swift/language/grammar/method/2020/05/03/Methods.html)
