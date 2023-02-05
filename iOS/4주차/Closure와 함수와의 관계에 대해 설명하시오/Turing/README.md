# [2023.02.01] Closure와 함수와의 관계에 대해 설명하시오

정답 : 함수도 클로저이다. 

우리가 지금까지 func로 선언한 모든 것은 클로저이다.

이말이 뭐냐고?

클로저(Closure) 에는 두가지가 존재함

1. **이름이 있는 클로저 (Named Closure)**
2. **이름이 없는 클로저 (Unnamed Closure)**

**이름이 있는 클로저(Named Closure) 를 우리는 지금까지 함수**로 불러왔던 것임 !! 

그러니까 우리가 말하는 클로저는 (Unnamed Closure)를 의미함

그럼 클로저에대해 좀 알아보자

1. 클로저는 사용자 코드안에서 전달되어 사용할 수 있는 **로직 → 괄호 {}로 구분 된 코드블럭**이다.
2. **일급 객체다**. 변수/상수 등으로 저장하거나 전달 가능, 함수의 파라미터, 반환 값으로도 가능
3. 참조 타입이다.

- 클로저의 표현 방식

```swift
{ (인자) -> 반환타입 in 
 로직 구현
}
```

우리는 이처럼 **인자가 들어가 있는 클로저를 Inline Closure**이라고 부름

자그럼 1번의 클로저는 괄호 안에 {} 로 구분 된 코드블럭이 해결 될거임

- **1급 객체로의 클로저**
    
    진짜 1급 객체라는 말 자체가 어려움.. 1등급이랑 나는 거리가 멀어서 더 그런 듯
    
1. 클로저는 변수와 상수에 대입 할 수 있다.

```swift
let youngjaeName = {() -> () in 
		print("Choi_Young_Jae)
}

let printName = youngjaeName
```

1. **함수의 파라미터, 반환값으로 전달 가능**

```swift
func printingName(closure: () -> ()) {
			closure()
}

printingName(closure: { () -> () in 
		print("YoungJae") 
})

func printingName2() -> () -> () {
	
	return { () -> () in 
				print("youngJae")
	}
}
```

- 정의 : 함수의 마지막 파라미터가 클로저일 때, 마지막이라고 했다잉! 이를 파라미터의 값이 형식이 아닌 함수뒤에 붙여서 작성하는 문법
- **트레일링 클로저(Trailing Closure)**
    
    ```swift
    // 인라인 클로저
    printingName(closure: { () -> () in 
    		print("youngJae") 
    })
    
    // 트레일링 클로저 - closure 생략 (마지막 파라미터가 클로저일 때)
    printingName { () -> () in 
    	print("YoungJae) 
    	}
    }
    ```
    

- **클로저의 경량문법**

 클로저를 줄여쓰는 방법임.. 정말 애플이란..

```swift
func doSomething(closure: (Int, Int, Int) -> Int) {
    closure(1, 2, 3)
}

doSomething(closure: { (a: Int, b: Int, c: Int) -> Int in
    return a + b + c
})
```

1. 파라미터의 형식과 리턴 형식을 생략할 수 있다.

```swift
doSomething(closure: { (a, b, c) in
    return a + b + c
})
```

1. Parameter Name은 Shortand Argument Names으로 대체
    - 여기서 Parameter Name은 a, b ,c 임 → 이걸 인덱스 형식의 $0, $1, $2로 치환 가능

```swift
doSomething(closure: {  
    return $0 + $1 + $2
})
```

1. 코드안에 단일 리턴문만 남을 경우 그마저도.. 생략 가능

```swift
doSomething(closure: {  
     $0 + $1 + $2
})
```

- **오토 클로저 (@autoclosure)**

 오토클로저는 클로저는 이름에 힌트가 있는데 오토(자동으로) 클로저를 만들어준다는 의미

아니.. 클로저를 왜 자동으로 만들지? 이런 의문이 먼저 든다.

일반구문을 오토클로저 키워드를 붙이면 클로저로 만들어준다는 것임

**An *autoclosure* is a closure that’s automatically created to wrap an expression that’s being passed as an argument to a function.** 

준내.. 이해가 안가지만 

1. 파라미터가 없는 !! 없어야함 리턴값은 상관없구 표현식을 클로저로 변환한다.

 2.  일반구문을 클로저 형태로 실행한다

1. 바로 실행 구문이 지연실행 된다.

결국에는 코드의 편의성을 위해 중괄호를 없애고 일반 구문으로 넣어주겠다는 거임.. 

그리고 지연구문 실행이 필요할 때 사용한다는 것이다. 자 이제 코드로 레츠기

일단 일반 구문을 클로저 형태로 실행하는 예제 ㄱㄱ 

```swift
// 일반 클로저
func f( pred: () -> Bool ) {
  if pred() {
    println("It`s true")
  }
}

f({ 2 > 1 } )

// 오토클로저
func g(@autoclosure pred: () -> Bool ) {
  if pred() {
    println("It`s true")
  }
}

g( 3 > 1 )
//or
g( { 3 > 1 }() )
```

지연속성 관련 paka님이 잘 설명해 주셔서.. 굳굳.. 한번에 이해됐다

```
var arr = [ 1,2,3]
var neverWorksBeforeCalled = {arr.remove(at:0) }
print(arr.count) // 3 출력,  클로저로 감싸여 있기 떄문에 호출전까지는 평가가 지연됩니다.
neverWorksBeforeCalled()
print(arr.count) //2 출력, 호출되어 내부의 표현식이 평가됩니다.
var worksImmediately = arr.remove(at:0) 
print(arr.count) // 1 출력, 클로저로 감싸지 않은 표현식은 즉시 평가됩니다.

// 즉 클로저로 감싸게 되면 클로저 호출 전까지는 변하지 않지만 
// 일반 표현식은 즉시 실행되어 값이 수정됨을 알 수 있다..
```
결국 지연속성은 프로그램의 메모리에 언제 올릴지 코더에게 제어권을 가져올 수 있다는 것이다.


- @escaping

말 그대로 클로저를 실행만 시키는 것이 아닌 탈출시켜 보자!! 이런 의미

```swift
func doSomething(closure: () -> ()) {
    let f: () -> () = closure //에러 발생
}
```

그래서 함수안에서 상수나 변수로 클로저를 받거나 함수가 끝난 후에도 클로저를 실행하고 싶을 경우 

@escaping 키워드를 사용하면 된다는 것이다.

```swift
func doSomething(closure: @escaping() -> ()) {
    let f: () -> () = closure
}
```

끝..
