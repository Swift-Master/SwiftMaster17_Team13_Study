# Generic에 대해 설명하시오.

### **제네릭이란?**

- 유연하고 재사용성 높은 코드를 작성할 수 있다.

- Swift 표준 라이브러리 대부분은 Generic으로 작성 되어있다.

- Swift의 Array 및 Dictionary 유형은 Generic 컬렉션이다.

<br>

```swift
func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}

var a = 2
var b = 5

swapTwoDoubles(&a, &b) 
// cannot convert value of type 'Int' to expected argument type 'Double'
```

`swapTwoDoubles` 의 파라미터는 `double` 형이기 때문에 `int` 값인 2와 5를 넣으면 오류가 생긴다.

해결하기 위해 타입마다 함수를 따로 만들어줘야 하는데 이것을 편하게 하기 위해 generic을 사용한다.

<br>

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

이렇게 함수명 뒤에 `<T>` 를 붙혀주고 파라미터 자리에 `< >` 안에 있는 값을 써준다.

<br>

여기서 `T` 는 타입 파라미터이다 !

<br>

그냥 T라는 이름을 가진 타입 파라미터이다.

다른 이름으로도 변경이 가능하다.

ex) `func swapTwoValues<je>`

<br>

그리고 타입 파라미터는 함수가 호출될때마다 타입이 결정된다.

<br>

❗️ a와 b는 T 타입이 반드시 일치해야 한다 ❗️

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}

var a = 2
var b = 5.0

swapTwoValues(&a, &b)
```

이렇게 사용하면 안된다는 것이다..!

<br>

```swift
var dic = [1: "youtube", 2: "facebook", 3: "instagram"]
for (num, plat) in dic {
   print(num, plat) 
}
```

`dictionary` 의 for문에서 타입 파라미터 (key, value)값을 (num, plat)으로 변경하여 사용해도 된다.


<br><br><br>

++ 질문에 대한 답변

Q. 제네릭에서 함수가 호출되는 시점은 런타임인가요?
<br>
A. 제네릭은 컴파일 타임에 타입이 결정되고(컴파일러가 타입을 결정), <br>애플리케이션 실행 시 런타임에 호출되는 함수에서는 제네릭 타입이 결정된 상태로 동작합니다.

