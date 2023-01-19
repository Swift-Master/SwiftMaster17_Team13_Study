# Closure에 대하여 설명하시오.

### **클로저란?**

- func 키워드를 이용해 이름을 붙여주는 함수

<br>

클로저에는 두가지 종류가 있다.

1. **Named Closure**
2. **Unnamed Closure**

<br>

우리가 여태 써왔던 이름이 있는 함수는 이런 **Named Closure**이고, 우리는 그냥 **함수**라고 불렀던 것이다.

```swift
func doSomething() {
    print("Named Closure")
}
```


<br>

그리고 이름을 붙이지 않고 사용하는 함수를 

**익명함수**, 즉 **Unnamed Closure** 라고 부르는 것이다.

```swift
let closure = { print("Unnamed Closure") }
```

<br>

정리하자면 !!

<br>

**클로저는 Named Closure & Unnamed Closure 둘다 포함하지만, 보통 Unnamed Closure 를 말한다.**

<br>

---

<br>

### **클로저 표현식**

<br>

```swift
{ (Parameters) -> Return Type in
    실행 구문
}
```

이렇게 표현식을 사용하는데, 익명 함수인 만큼 `func` 이란 키워드를 사용하지 않는다.

그리고 클로저는 **클로저 헤드**와 **클로저 바디**로 이루어져 있는데

<br>

```swift
{ (Parameters) -> Return Type in
    실행구문
}

// Closure Head : (Parameters) -> Return
// Closure Body : 실행구문
```

이 둘을 구분지어주는게 바로 `in` 이라는 키워드이다.

<br>

### **☝ Parameter와 Return Type이 둘 다 없는 클로저**

<br>

클로저는 익명이긴 하지만 **함수**이다.

따라서 Swift에서 1급 객체이기 때문에 상수에 클로저를 대입할 수 있다.

특히 **Parameter** 와 **Return Type**이 둘 다 없는 경우는 다음과 같이 사용함

<br>

```swift
let closure = { () -> () in
    print("Closure")
}
```


**Return Type** 이 **있어도, 없어도 생략 가능**하고 **Parameter** 조차 생략 가능하다.

<br>

### **✌ Parameter와 Return Type이 있는 클로저**

<br>

```swift
let driving = { (place: String) -> String in
    return "저는 차를타고 \(place)에 가고 있습니다."
}
```

**Parameter**와 **Return Type**이 있는 경우는 이렇게 표시한다.

<br><br>

**함수 때 배운 대로 Parameter의** `place`**는 단독으로 쓰였으니, Argument Label이자 Parameter Name**이라고 생각할 수 있지만,

<br>

⭐ **클로저에선 Argument Label을 사용하지 않는다** ⭐

<br>

따라서, name은 **Argument Label이 아니고**, 오직 **Parameter Name** 이다. 

클로저를 호출할 때는 **Argument Label을 사용하지 않는다 !!**

<br>

```swift
driving("회사") // 저는 차를 타고 회사에 가고 있습니다.
driving(place: "회사") // Error!
```

사용할 시 에러가 난다….!

<br>

---

<br>

### **1급 객체로서 클로저**

<br>

`1. 클로저를 변수나 상수에 대입할 수 있다.`

⇒ 클로저를 변수나 상수에 대입할 수 있고, 대입된 변수나 상수를 실행 시키는 것이 가능하다.

```swift
let closure = { () -> () in
    print("Closure")
}

let closure2 = closure
```

<br>

`2. 함수의 파라미터 타입으로 클로저를 전달할 수 있다.`

```swift
func doSomething(closure: () -> ()) {
    closure()
}
```

이런 식으로 **함수를 파라미터로 전달받는** **doSomething** 이라는 함수가 있는데 

파라미터로 **함수**를 넘겨줘도 되지만

<br>

```swift
doSomething(closure: { () -> () in
    print("Hello!")
})

// 클로저로 작성된 부분 : { () -> () in print("Hello!") }
```

이렇게 **클로저**를 넘겨줘도 된다.

doSomething이란 함수에서 파라미터로 전달받은 함수를 실행시키면 

작성한 클로저가 실행된다.

<br>

`3. 함수의 반환 타입으로 클로저를 사용할 수 있다.`

```swift
func doSomething() -> () -> () {

    return { () -> () in
            print("Hello JE!")
    }
}
```

선언부는 기존 함수와 똑같지만 `return`할 때 실제 값을 함수가 아닌 **클로저를 리턴** 할 수 있다.

<br>

```swift
let closure = doSomething()
closure() // Hello JE!
```

또한 호출하는 곳에서 클로저를 받아서 이렇게 실행시킬 수 있다.

<br>

---

<br>

### **클로저 실행**

<br>

- 클로저가 대입된 변수나 상수로 호출

```swift
let closure = { () -> String in
    return "Hello JE!"
}

closure()  // Hello JE!
```

이런 식으로 클로저가 대입된 상수 closure를 호출 구문인 ()를 이용해서 실행시킬 수 있다.

<br>

- 클로저를 직접 실행하기

```swift
({ () -> () in
		print("Hello JE!")
})()  // Hello JE!
```

클로저를 변수나 상수에 대입하지 않고 실행시키고 싶다면, (완벽한 일회성)

그땐 **클로저를 ( )로 감싸고, 마지막에 호출 구문인 ( )를** 추가해주면 된다.

<br><br><br><br>

### 📖 **참고**

---

[https://babbab2.tistory.com/81](https://babbab2.tistory.com/81)
