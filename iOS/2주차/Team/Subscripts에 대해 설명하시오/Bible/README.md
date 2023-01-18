# Subscripts에 대해 설명하시오.

### Subscripts란?
서브스크립트는 콜렉션, 리스트, 시퀀스 등 집합들을 이루고 있는 요소들에 간단하게 접근할 수 있는 문법입니다.  
서브스크립트를 이용하면 추가적인 메서드를 사용하지 않고 값을 할당하거나 가져올 수 있습니다.  

예를들어,
```swift
var array: Array<Int> = [1, 2, 3]

// 0번째 요소에 있는 값을 4로 재할당
array[0] = 4

// 2번째 요소 값 가져오기
var arrayIndex2 = array[2]
```
array라는 배열 변수에 1, 2, 3 총 3개의 정수 요소들이 있을 때, 대괄호(서브스크립트)를 이용하여 각 요소에 접근할 수 있습니다.  

### Swift의 String은 Subscripts로 접근이 안 되는 이유
Java나 Python 등 여러 언어에서는 문자열을 다룰 때 서브스크립트를 사용해서 문자열의 문자 하나하나를 접근해서 사용할 수 있습니다.  
```java
String str = "JAVA";
String strIndex0 = str[0] // J
```
Java 코드로 예시를 들면, **JAVA**라는 문자열이 있을 때 서브스크립트를 사용하면 마치 배열처럼 [J, A, V, A] 0번째는 J, 1번째는 A, 2번째는 V, 3번째는 A로 각 요소에 접근해서 값을 쉽게 사용할 수 있게 됩니다.  

하지만 Swift에서는 이러한 사용이 불가능합니다.  
  
애플에서 String은 "A Unicode string value that is a collection of characters."  
Character의 집합인 유니코드 문자열이라고 설명합니다.  
  
여기서 유니코드는 다양한 언어와 문자를 표현하기 위한 방식 중 하나입니다.  
유니코드와 같이 아스키 코드도 있는데 아스키 코드는 1byte(8bit)의 크기를 가진 문자를 표현 방식입니다.  
아스키 코드로는 128개의 문자 밖에 표현할 수 없어서 나온 것이 유니코드입니다.  
유니코드로는 다양한 언어와 이모티콘까지 표현이 가능합니다.  
또한 유니코드는 거의 모든 문자를 컴퓨터가 알아볼 수 있도록 인코딩이 가능합니다.  
이 인코딩 방법에는 UTF-8, UTF-16과 같은 방식이 있습니다.  
  
다시 Swift의 String, Character를 이루고 있는 Unicode Scalar란,
- 크기가 가변적인 String 문자열을 하나하나 개별적으로 접근하기 위한 방법.
- Unicode 기반 21-bit 코드
- UTF-32와 거의 동일
- 하나 이상의 Unicode Scalar가 모여 Character를 이룸

크기가 가변적이란 말은 다음 코드로 예시를 들 수 있습니다.   
```swift
let testStr1 = "P"
print(testStr1.utf8.count)              // 1
print(testStr1.utf16.count)             // 1
print(testStr1.unicodeScalars.count)    // 1

let testStr2 = "편"
print(testStr2.utf8.count)              // 3
print(testStr2.utf16.count)             // 1
print(testStr2.unicodeScalars.count)    // 1

let testStr3 = "片"
print(testStr3.utf8.count)              // 3
print(testStr3.utf16.count)             // 1
print(testStr3.unicodeScalars.count)    // 1

let testStr4 = "👍"
print(testStr4.utf8.count)              // 4
print(testStr4.utf16.count)             // 2
print(testStr4.unicodeScalars.count)    // 1
```
단일 문자에 대해서 각각 UTF-8, UTF-16, Unicode Scalar의 길이를 출력해보았을 때  
Unicode Scalar는 알파벳, 한글, 한자, 이모지 어느 것에서든 항상 길이가 1이지만,  
UTF-8과 UTF-16의 길이는 가변적으로 변하는 것을 볼 수 있습니다.  
  
하지만! 모든 단일문자의 Unicode Scalar 길이가 1인것은 아닙니다.  
```swift
let testStr5 = "👯‍♂️"
print(testStr5.utf8.count)              // 13
print(testStr5.utf16.count)             // 5
print(testStr5.unicodeScalars.count)    // 4

let testStr6 = "👨‍👩‍👦"
print(testStr6.utf8.count)              // 18
print(testStr6.utf16.count)             // 8
print(testStr6.unicodeScalars.count)    // 5
```
이상하게도 위의 두 이모지의 Unicode Scalar 길이는 각각 4, 5로 나옵니다.  
그럼 다시 두 이모지를 대입한 변수 자체를 다시 count 해보면,  
```swift
let testStr5 = "👯‍♂️"
let testStr6 = "👨‍👩‍👦"

print(testStr5.count)   // 1
print(testStr6.count)   // 1
```
문자열을 바로 count하면 1이 나오는 걸 확인할 수 있습니다. 당연한 결과죠! 문자열 문자가 1개 있으니까요.  
즉 하나 이상의 Unicode Scalar가 모여서 Character를 만들고, Character가 모이면 String이 되는 것입니다.  

또한 Swift는 이 Character를 나타낼 때 단일 유니코드도 사용하고 여러 개의 유니코드를 합쳐서 하나의 문자를 표현하기도 합니다.   
이 말은 같은 문자를 다른 방식으로도 표현이 가능하다는 것입니다.  
(Ex. \u{D55C} = "한" 이지만, \u{1112}\u{1161}\u{11AB} = "ㅎ + ㅏ + ㄴ"으로 똑같이 "한"을 표현)  
때문에 이 문자를 위해서는 메모리 공간도 가변적으로 필요해집니다.  
  
다시 원래의 질문으로 돌아와 Swift의 String이 왜 서브스크립트를 지원하지 않냐면,  
동일한 크기를 가지는 아스키 코드를 사용하는 다른 언어와 다르게 유니코드를 사용하는 Swift의 문자 크기는 가변적이기 때문에 Int를 통한 특정 Index 글자 접근이 불가능하기 때문입니다.  
