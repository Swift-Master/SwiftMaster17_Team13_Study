# Extension 내부에서 함수를 override할 수 있는지 설명하시오.

## Extension
- 기존 클래스, 구조, 열거형 또는 프로토콜 유형에 새로운 기능을 추가할 수 있습니다.
- 주로 프로토콜의 구현 메서드들을 Extension에서 구현합니다.
- [Swift 공식 가이드북](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html)에는 `확장은 유형에 새 기능을 추가할 수 있지만 기존 기능을 재정의할 수는 없습니다.`라고 나와있습니다.

## 그러나 실제로 Extension 내부에서 문제없이 override를 사용할 수 있습니다
![예제1](./override%EA%B0%80%EB%8A%A5%EC%98%88%EC%A0%9C.png)
### 왜?
- 순수 Swift 프레임워크(?)라면 당연히 가이드북에 나온대로 사용할 수 없겠지만, 대부분의 객체들은 Objective-C와 상호작용합니다. 
- Objective-C와 상호작용이 존재하는 객체에 한해서 메서드를 Extension에서도 재정의가 가능합니다!

![예제2](./%EC%98%B5%EC%A0%9DC%EC%83%81%ED%98%B8%EC%9E%91%EC%9A%A9.png)

### 그래도 사용은 자제하자...
- Extension의 역할은 새로운 기능을 추가하는 것이지, 기존의 기능을 수정하는 것이 아닙니다.
- 물리적으로 가능할 뿐, 좋지 않은 코드입니다.

## 참고
- [Swift 공식 가이드북 - Extension](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html)
- [스택오버플로우(Overriding methods in Extension)](https://stackoverflow.com/questions/38213286/overriding-methods-in-swift-extensions/38270173)
