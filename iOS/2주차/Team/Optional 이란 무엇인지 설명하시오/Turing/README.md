# [2023.01.19] 옵셔널(Optional)

### 스위프트의 🌻  옵셔널에대해서 알아보자

```jsx
let name: String?
```

스위프트에서 옵셔널이 꽃이냐면 스위프트에서 도입된 새로운 개념이기 때문

자 그러면 옵셔널을 왜 사용하냐 이세글자가 가장 중요해 **“안정성”** 

그럼 거두 절미하고 

옵셔널이 가질 수 있는 값은 두가지야 :)

1. **nill이 아닌 값**
2. **nill 값**

nill이 아닌 값? 그럼 “apple”, 123 등 구체적인 값을 말하는거야? 라는 질문이 들 수 있지

하지만 여기서는 반환하려는 실제 값을 옵셔널이라는 객체로 감싼 값을 의미해 

```jsx
Optional(123)
```

자 다시 위에 **안정성**에 대해 말해보자. 스위프트는 값을 반환하는 과정에서 오류가 발생할 가능성이 있는 값을 옵셔널 타입으로 감싸서 앱의 구동이 멈추는 일을 최소화 하고 싶은거야!

우리는 이를 옵셔널 래핑(Optional Wrapping)이라고 해

그래서 예가 뭔데

```jsx
1. Int("123")
2. Int("Youngjae")
```

그래 너가 생각하고 있는게 맞아.. 1번은 성공할 것 같은 느낌이오지?  근데 2번은..? 저 문자열을 숫자로 만들 수 있을까

그렇다고 반환값에 “죄송해유.. 변환할 수 없어유” 할 수는 없잖아!

그래서 옵셔널이 존재한다고 생각해 안정성은 아래서 좀 더 이야기 해줄게

근데 우리는 결국 옵셔널 래핑을 벗기고 싶고 nill이 아닌 값을 Optional(123) → 123 이렇게 사용하고 싶잖아

그럼이제 래핑을 언래핑해보자구!

### 옵셔널 언래핑 (Optional Unwrapping)

1. 강제 언래핑 (Forced-Unwrapping Operator)
2. 비강제 언래핑 (Unforced-Unwrapping Operator)

먼저 강제 언래핑 기기

누구든 강제로 나한테 뭘 시키면 더 하기 싫잖아 그래서 실무에서도 잘 안쓰여.. 근데 편하긴하지

```jsx
var name: String = "youngjae" 
var optionalName: String? = "youngjae" 

print(name) //youngjae
print(optionalName) //Optional("youngjae")
print(optionalName!) //youngjae
```

 ! 연산자만 붙이면 바로 풀려버려 겁나 편하다 그치?

그런데.. nill 일때 강제해제하면 뭐가 나와야 한다고 생각해? 한번 보자

![Untitled](https://user-images.githubusercontent.com/85090866/213413555-ed17357d-2597-4e38-afe1-5213550bc673.png)

엄청난 오류가 떠버려 실제 앱이였다면 끔찍하군 🙂

근데 이렇게 컴파일 에러가 떠주는건 정말 다행이야 실제였다면 비정상적으로 종료가 될거고 

우리는 그래서 두번째인 **비강제 언레핑, 비강제 해제, 옵셔널 바인딩을 쓰는거야 “안정성”** 잊지 말라구

미리 말해야겠어 비강제 해제는 세가지가 있어

1. **if let** 
2. **guard let**
3. **while let**

갑자기 이게 뭐지 할 수 있겠지만 하나하나 봐보자

1. 옵셔널 표현식을 먼저 조건 비교한다
2. 값이 있는경우 옵셔널이 해제된 값을 let(상수)에 저장하고 True를 반환한다
3. 값이 없으면 false를 반환한다.
4. 타입추론이 되므로 Type Annotation은 대부분 생략한다 : String 이론고~

```swift
var testName: String? = "Young Jae"

if let name = testName {
    print(name)
} else {
    print(testName)
}

// Young jae
```

```swift
let testName: String? = "Young Jae"

guard let name = testName else { return }
```

다른 점은 guard let은 else가 바로 붙는다.  guard let은 함수, 메서드, 반복문 안에서만 사용이 가능하기 떄문이야 else 블록은 return, break, throw 등을 사용할 수 있지

우리는 이런 If, guard에 익숙하니까 **옵셔널체인**으로 바로 들어가보자

```swift
struct Human { 
	var name: String?
	var man: Bool = true
}

var boy: Human? = Human(name:"홍길동", man:true)
```

일단 Human에 옵셔널 타입으로 선언된 이상, 변수 boy를 사용하려면 언래핑을 해야하는거지?

그래야 안정성이 유지되니까

```swift
if let b = boy {
	if let name = b.name {
		print("이름은 \(name)입니다")
	}
}
```

변수가 많아지고 코드가 길어지면..? if문의 중첩이 길어지고 코드 작성해야할 입장에서 부담이겠지?

### 옵셔널체인

```swift
struct Human {
    var name: String?
    var man: Bool = true
}

var boy: Human? = Human(name:"홍길동", man:true)

// 1번
if let b = boy {
    if let name = b.name {
        print("이름은 \(name)입니다")
    }
}

// 2번
var name2 = boy?.name
print("이름은 \(name2!)입니다.")
```

옵셔널 체인은 if 구문을 쓰지 않고 간결하게 사용하기 위해 도입되었어

그럼 좀 더 복잡한 옵셔널 체인을 볼까?

```swift
struct Human {
    var name: String?
}

struct Person {
    var man: Human?
    var woman: Human?
}

var soomin: Person? = Person(man: Human(name: "임수민"))
var namePrint = soomin?.man?.name
print(namePrint!) //임수민
```

간단하게 접근 가능한데 특징만 보고 마무리하자

1. 옵셔널 체인으로 참조된 값은 무조건 옵셔널 타입으로 반환된다.
2. 옵셔널 체인과정에서 옵셔널 타입들이 여러 번 겹쳐 있더라도 중첩되지 않고 한 번만 처리된다.

1번은 쉬우니까 2번을 조금 설명하자면

```swift
Optional(Optional(Optional("youngjae"))) = Optional("youngjae")
```

우리가 옵셔널이 가질 수 있는 값은 nil이 아닌값 nil인 값이라고 했지? 그말이야! 

즉 옵셔널은 중첩이되더라도 단하나의 옵셔널로 감싼 값과 같다.
