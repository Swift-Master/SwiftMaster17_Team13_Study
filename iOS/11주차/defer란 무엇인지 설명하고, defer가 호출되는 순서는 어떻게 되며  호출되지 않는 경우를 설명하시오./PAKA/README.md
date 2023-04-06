
# defer란 무엇인지 설명하고, defer가 호출되는 순서는 어떻게 되며 호출되지 않는 경우를 설명하시오.

  
## 특징

- Swift 2에 도입, 현재 블록의 가장 마지막에 클로저 코드 구문을 실행하는 연산자입니다.

- 메서드에 많은 반환 문이 있고 각 문 앞에 동일한 코드를 복사하여 붙여넣고 싶지 않은 경우에 유용합니다.

- defer에 넣는 코드는 일반적으로 함수가 시작될 때 리소스를 할당하거나 캡처하는 명령과 함께 제공되는 정리 루틴입니다.

 ## 호출 순서 

### 예시 1

```swift

var a = "Hello"

  

func  b() -> String {

defer { a.append(" world") }

return a

}

```

  

```swift

func  d() -> String {

a.append(" world")

return a

}

```

위에서 본 특징대로, b와 d의 반환값은 다르다는 것을 알고 있습니다.

```swift

a = "Hello"

print(b()) //Hello
print(a) // Hello world
a = "Hello"

print(d()) //Hello world

```
 출력을 통해 증명하였으나 이것은 일반적인 프로그래밍 원리와 역행하는 결과입니다🤔

- 모든 프로그래밍 언어의 모든 기능은  return 문으로 끝납니다. return 시 코드 영역에서 벗어나 스택에서 함수를 `pop`하고 호출 계층에서 한 단계 위로 중단된 위치로 이동합니다.

- Objective-C의 `autorelease` 는 고유의 런타임 런루프에서 `autorelease pool`을 통해 return 이후에도 리소스를 해제할 수 있으나, 조금 다른 원리입니다.

한번 컴파일 과정의 sudo 코드를 보면서 실제 동작원리를 살펴봅시다. 코드보다 아래 설명을 읽으면서 보는 것이 더 좋습니다🔄

```swift

// b()의 sudo 코드

int _$S05test_A01bSSyF() {

swift_beginAccess(_$S05test_A01aSSvp, &var_18, 0x20, 0x0);
// 레지스터에 전역변수 값을 저장 (캡쳐)
rcx = *_$S05test_A01aSSvp;

swift_bridgeObjectRetain(rcx);

swift_endAccess(&var_18);

$defer #1 (); //defer 클로저 호출

rax = rcx;

return rax;

}

// defer문의 클로저 sudo 코드

int _$S05test_A01bSSyF6$deferL_yyF() {

var_40 = Swift.String_builtinStringLiteralutf8CodeUnitCountisASCII.init(" world", 0x6, 0x1);

swift_beginAccess(_$S05test_A01aSSvp, &var_20, 0x21, 0x0, &var_20, 0x21);

$SSS6appendyySSF(var_40, 0x1);

swift_endAccess(&var_20);

rax = swift_bridgeObjectRelease(var_40);

return rax;

}
```

swift_beginAccess 및 swift_endAccess 사이의 코드는 전역 변수 a를 내부 영역에 저장하는 과정입니다.

중요한 부분은 3, 6, 7 행입니다. 
1. 3번 행에서 a의 초기 값은 rcx 레지스터에 저장됩니다. 
2. 6번째 줄에서 defer문을 호출합니다. defer는 새 문자열 "world"를 생성하고 전역변수 a 원본에 연결합니다. 이후 완성된 "hello world" 문자열을 반환합니다.
3. 재밌게도, defer 호출 전 1번에서 저장해두었던 "hello"를 "hello world"가 할당되어 있던  레지스터 rax에 할당하여 최종적으로 "hello"를 반환하게 됩니다.

defer를 사용하지 않는다면, 이렇게 지역변수를 선언해서 같은 동작을 구현해야 할껍니다.

```swift

func  c() -> String {

let d = a

a.append(" world")

return d

}

```

이런 약간의 마법🧙🏻‍♂️(?)을 통해 실제로 return 이전에 실행되지만, return 이후에 수행되는 것처럼 동작하게 됩니다!

  

### 예시2

  

```swift

var a: String? = nil

  

func  b() -> String {

a = "Hello world"

defer { a = nil }

return a!

}

  

print(b())

```

이번 예시는 함수의 마지막에 defer문이 실행되는 것을 이용하여 전역변수를 사용 후 다시 초기화시켜주는 가장 보편적인 사용법입니다. 

그러나, 여기서도 한가지 의문점이 생깁니다.
코드를 봤을 때, 마지막에 사용한 전역변수에 대해 강제 언래핑을 수행하고 있습니다. 
예시 1의 컴파일 코드를 기억하시나요? 분명 defer문은 리턴하기 전에 실행되기 때문에 실제 리턴이 발생할 때 a = nil이기 때문에 런타임 에러가 발생해야 합니다.
  

```swift
//컴파일 과정 수도 코드입니다. 처음부터 코드를 다 읽으려 하지 마시고 아래 설명과 함께 보시는 걸 추천
  

int _$S10test_force1bSSyF() {

// 변수 a에 Hello World 문자열 할당 과정 시작
var_40 = Swift.String_builtinStringLiteralutf8CodeUnitCountisASCII.init("Hello world", 0xb, 0x1);

swift_beginAccess(_$S10test_force1aSSSgvp, &var_18, 0x21, 0x0, 0x21);

rdi = *_$S10test_force1aSSSgvp;

rsi = *qword_100001078;

*_$S10test_force1aSSSgvp = var_40;

*qword_100001078 = 0x1;

_$SSSSgWOe(rdi, rsi);

swift_endAccess(&var_18); 
// 변수 a에 Hello World 문자열 할당 과정 끝

// 전역 변수 a를 읽어들여 지역변수로 저장. 예시 1에서 했던 과정
swift_beginAccess(_$S10test_force1aSSSgvp, &var_30, 0x20, 0x0);

rax = *_$S10test_force1aSSSgvp;

rcx = *qword_100001078;

var_48 = rax;

var_50 = rcx;

_$SSSSgWOy(rax, rcx);

swift_endAccess(&var_30);
// 전역 변수 a를 읽어들여 지역변수로 저장 끝.

// 저장된 지역변수를 언래핑하는 과정. 성공시 defer를 호출합니다.
if (var_48 != 0x0) {

var_58 = var_48;

var_60 = var_50;

$defer #1 ();  // defer 호출

rax = var_58;

}
// 실패시 충돌이 발생합니다.
else {

stack[-168] = "test_force.swift";

*(int32_t *)(&stack[-168] + 0x20) = 0x1;

*(&stack[-168] + 0x18) = 0x6;

*(int32_t *)(&stack[-168] + 0x10) = 0x2;

*(&stack[-168] + 0x8) = 0x10;

Swift_fatalErrorMessage first-element-marker first-element-marker fileline.flags("Fatal error", 0xb, 0x2, "Unexpectedly found nil while unwrapping an Optional value", 0x39, 0x2);

asm { ud2 };

rax = loc_100000d90();

}
// 최종 리턴
return rax;

}

```

전역변수 a에 `Hello World` 를 할당하는 2~9 행과 defer를 호출하는 20번 행을 제외하면 모두 return a! 연산 과정입니다. return a! 과정은 간략하게 3단계로 이루어집니다.

1. a 값을 읽고 로컬 범위에 저장합니다. 이는 10행과 16행에서 swift_beginAccess/swift_endAccess의 두 번째 쌍 사이에서 발생합니다.

2. 지역변수로 저장된 a값을 언래핑합니다. 17행과 32행 사이의 if 문이 바로 이것입니다. 18~21행은 값이 존재할 때 성공적인 결과를 나타내고, 24~31행은 nil 값으로 판별 되었을 때 충돌을 발생시킵니다.

3. 이후 결과를 리턴합니다.

색으로 그룹화된 코드는 다음과 같습니다.
  

![lldb? 코드](https://user-images.githubusercontent.com/116094622/230354472-3d72de20-e917-457b-a894-9c7ebf42b76b.png)

같은 동작을 하는 컴파일 단 코드와 맵핑된 우리가 보는 코드 영역입니다.
![실제 코드](https://user-images.githubusercontent.com/116094622/230354494-99309d00-a4c6-4db7-a08b-846647e5b2a3.png)

이처럼 컴파일 과정에서 강제 언래핑 `!`  로직 내부(성공 프로세스)에 defer 호출이 삽입되어 있기 때문에, 적어도 defer문의 클로저 코드에 의해 충돌이 발생하는 일은 없을 것입니다👍
 
 ### 결론
 defer문을 통해 return문 내부 동작을 조금 더 세밀하게 구현할 수 있었습니다.
 블록내의 영역에서 레지스터에 전역변수 값을 저장하는 동작도 함께 수행하게 되기 때문에, 불필요한 지역변수 작성을 예방할 수 있습니다!
 
 ## defer가 호출되지 않는 경우?
 위의 예시에서 살펴봤듯이, return 이전에 동작하기 때문에 return 이후에 defer를 작성하면 동작하지 않을 것입니다!
 
 ```swift
 func testDefer() {

print("Check #1")

return;

defer { print("defer #1") }

print("Check #2")

}

testDefer() //defer #1, Check #2는 출력되지 않는다!
```

## 참고한 출처
- [Swift에서 defer문이 실제로 동작하는 방식](https://medium.com/@sergeysmagleev/how-defer-operator-in-swift-actually-works-30dbacb3477b)
- [defer를 알아보자](https://babbab2.tistory.com/80)