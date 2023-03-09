# String은 왜 subscript로 접근이 안되는지 설명하시오.


- String 정의 : A **Unicode string** value that is a collection of characters.

```xml
A string is a series of characters, such as "Swift", that forms a collection. Strings in Swift are Unicode correct and locale insensitive, and are designed to be efficient. The String type bridges with the Objective-C class NSString and offers interoperability with C functions that works with strings.
```

String은 characters의 collection이고 Unicode string 이기 때문에 안되는거야.

먼저 Unicode String이 뭔지 알아보자!

우리는 문자를 어떨게 출력하고 저장할지에 대한 약속을정했어 ⇒ 이걸 “인코딩” 이라고 부르지

처음 나온 건 **ASCII (American Standard Code for Information Interchange, 미국 정보 교환 표준 부호)**야 근데 요놈이.. 라틴 문자와 숫자, 몇몇 특수문자를 128개를 코드값에 1:1 대응시키는 법을 고안했는데

세계의 언어가 얼마나 많겠어 그래서 표현하기에 한계가 온거야 

그럼 새로운 인코딩 방식이 나오겠지?

그러다 나온게 **유니코드**인거지

**전 세계의 모든 문자를 컴퓨터에서 일관되기 표현하고 다룰 수 있도록 설계된 산업 표준** 

<img width="446" alt="스크린샷 2023-03-02 오후 5 42 12" src="https://user-images.githubusercontent.com/85090866/222381409-2b73ccba-fee4-4256-9264-6205c3afee2a.png">


이모지의 문자는 1개인데 .. 실제길이는 다르다는거야 그렇기 때문에 

보이는 [0] ⇒ robot 이모지가 나와야할 것 같지만 실제로는 길이가 달라 Subscript 할 수 없다는 거지 

Unicode 안으로 들어가보면  채택하고 있는 열거형으로 정의 되어 있는 걸 볼 수 있지.

<img width="491" alt="2" src="https://user-images.githubusercontent.com/85090866/222381502-e5de7c2c-da5f-4da3-a6d0-5e9f3c37915c.png">


그럼 왜? Swift는 유니코드를 채택했을까?

Swift에서는 유니코드를 호환해 String과 Character에서 텍스트 처리를 빠르게 처리한데

Swift의 모든 문자열은 인코딩에 독립적인 유니코드 문자로 구성되어 있고 다양한 유니코드 표현의 문자에 접근할 수 있도록 지원합니다. (딱딱 그자체..) 여튼 이유는 이렇고

그럼 String을 Subscript 할 수 없을까?

물론 있지.. ㅎㅎ

extension해서 String을 extension하여 Array처럼 사용하면 된다는 사실~ 잊지말라구

<img width="582" alt="3" src="https://user-images.githubusercontent.com/85090866/222381685-ba4f6a63-1498-4577-8e59-116db86e3d46.png">

