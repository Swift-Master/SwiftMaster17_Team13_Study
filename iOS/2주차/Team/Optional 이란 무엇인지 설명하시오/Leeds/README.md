# Optional 이란 무엇인지 설명하시오.

### **Optional이란?**

nil을 사용할 수 있는 타입과 없는 타입을 구분하기 위함이며,<br>
nil을 사용할 수 있는 Type을 **Optional Type**이라 부른다.

<br>

쉽게 설명하자면

<br>

**‘nil’** 이라는 값을 가질 수 있으면 **Optional Type**이고, 
이것을 선언할 때 타입 옆에 **‘?’** 를 붙인다.

```swift
let name: String? // String? = Optional Type
let age: Int? // Int? = Optional Type
```

<br>

---
<br>

### **nil이란?**

- 변수에 객체가 할당되지 않은 상태 "**값이 없음**" 을 뜻한다.
    
    ⇒ nil과 null은 다르다!
    
    > Nil : 오류가 났지만, 앱 중단시키는 것 대신 nil을 돌려줄 테니 오류 난 것을 알려줄 때 사용
    <br>
    > Null : 어떠한 값도 가지지 않고 있다는 뜻

<br> 

```swift
let dictHuman = ["name": "je", "gender": "female"]

let name = dictHuman["name"] // "je"
let gender = dictHuman["je"] // nil
```

<br>

"**name**"이라는 **key**에 접근을 하면 "**je**"이라는 **value**가 잘 뜨지만,

"**je**"이라는 **key**에 접근을 했을 때 **dictHuman**에는 "**je**" 이라는 **key**가 존재하지 않기 때문에 nil이 뜸

<br>

---

<br>

### **Non-Optional Type과 Optional Type**

- nil은 오류가 발생했을 때 오류 대신 뱉는 값이라고 하였다.
    
    Q. 어떤 자료형이든 nil을 반환하고 저장 할 수 있을까?<br>
    A. NO !! 절대 불가능 !!!!

<br>

**nil**을 **저장**할 수 있는 건 오로지 **Optional**로 **선언된 자료형** 뿐이다.

**Non-Optional Type**은 선언과 동시에 초기화 해주던가 Type Annotation을 사용해야 된다.

⇒ 이 친구는 무조건 값을 가져야 한다 !! 값이 없으면 오류 발생 !!!!

<br>
<br>

**Optional Type**

1. Type Annotation 이용
2. Type Inference 이용
- 타입 추론, ⭐**초기화 되는 값이 무조건 Optional 자료형**⭐ 이여야 된다

```swift
// Type Annotation
var name: String?
name = nil // ?로 선언된 name에는 nil 저장 가능

// Type Inference
let a: Int? = nil
let b = a
print(b) // nil
```

<br>

아까 nil이란? 을 설명했을 때

```swift
let gender = dictHuman["je"] // nil
```

<br>

위 구문이 가능했던 이유는 dictHuman에서 value값을 가져오는 [](subScript) 의 원형을 보면

```swift
@inlinable public subscript(key: Key) -> Value?
```

<br>

subScript 리턴 값이 옵셔널로 선언되어 있다.

따라서 dictHuman[”je”]의 리턴 값은 옵셔널 자료형인거고, 

상수 gender는 타입 추론에 의해 옵셔널 자료형을 갖게 된 것이다.

<br>

---

<br>

### **Type**

<br>

```swift
var today: String
var weather: String?

print(type(of: today))    // String
print(type(of: weather))  // Optional<String>
```

- **Non-Optional Type**과 **Optional Type**의 자료형 결과값은 다르다
    
    ⇒ Non-Optional Type = String Type<br>
    ⇒ Optional Type = Optional Type
    

<br><br><br><br>

### 📖 **참고**

---

[https://babbab2.tistory.com/15](https://babbab2.tistory.com/15)