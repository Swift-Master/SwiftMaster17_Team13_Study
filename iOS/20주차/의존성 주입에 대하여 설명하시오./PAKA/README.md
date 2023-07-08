# 의존성 주입에 대하여 설명하시오.

## 의존관계와 의존성 주입

<p><image src = "https://github.com/AKAPUCH/TIL/assets/116094622/fb600a19-88aa-4473-a8f0-cec921e3aa56" width = 70%></image></p>

`출처 : Clint Jang님 Medium블로그`

reference counting에서 어떤 클래스가 프로퍼티로 다른 클래스를 가짐으로써 강한 참조가 발생하는 경우를 기억하시나요?
</br>
아래 HighClass는 NormalClass를 프로퍼티로 가지고 있고 이 때 HighClass는 NormalClass와 의존관계에 있다고 할 수 있습니다.
```swift
import UIKit

class NormalClass {
	var realNumber : Int = 1
}

class HighClass {
	var internalVariable = NormalClass()

}

let newInstance = HighClass()
```

의존성 주입은 외부에서 생성한 객체를 내부의 변수에 할당해주는 것입니다.
</br>
예시에서는 생성자를 통해 주입했으나, 이외에도 setter 메서드, 프로토콜을 활용한 인터페이스 주입 방법도 가능합니다.
```swift
import UIKit

class NormalClass {
	var realNumber : Int = 1
}

class HighClass {
	var internalVariable = NormalClass()

	init(with externalVariable : NormalClass) {
		self.internalVariable = externalVariable
	}
}

let newInstance = HighClass(with : NormalClass())
```

모순적이나 의존성 주입은 의존성을 주입만으로 완벽하지 않습니다 😅

## 의존성 분리

<p><image src = "https://github.com/AKAPUCH/TIL/assets/116094622/1ddb25eb-d222-4193-b02d-5380662b2d70" width = 70%></image></p>

`출처 : Clint Jang님 Medium블로그`

SOLID의 DIP를 활용하여 결합도를 높이는 의존관계를 분리시킵니다.

### SOLID 5원칙 - DIP(의존 역전 원칙)

</br>

추상화를 통해서 다른 5원칙 중 하나인 개방-폐쇄 원칙(기능 확장 허용 & 기존 코드를 수정❌)을 지원하는 원칙입니다.
</br>
의존 관계를 맺을 때, 변화하기 쉬운 낮은 수준의 모듈이 잘 변하지 않는 높은 수준의 모듈에서 정의한 추상 타입에 의존하도록 설계해야 합니다.

- 단, 소스코드 단계에서의 의존 역전일 뿐, 런타임 실행 단계에서는 의존 역전이 발생하지 않습니다.

### iOS에서 DIP 적용하기 - 프로토콜

</br>
프로토콜은 상세 구현보다는 필수 요구사항만을 정의함으로써 Java의 인터페이스처럼 추상화를 사용할 수 있게 됩니다.

</br>
하위 모듈이었던 NormalClass에 의존했던 HighClass는 추상화된 인터페이스 DIPInterface의 변화에 의존 및 제어의 주도권이 넘어갑니다.
</br>
이를 의존성 분리 또는 제어의 역전, IOC라고 합니다.

```swift
import UIKit

protocol DIPInterface : AnyObject {
	var realNumber : Int {get set}
}
	
class NormalClass : DIPInterface {
	var realNumber : Int = 1
}

class HighClass {
	var internalVariable : DIPInterface

	init(with externalVariable : DIPInterface) {
		self.internalVariable = externalVariable
	}
}

let newInstance = HighClass(with : NormalClass())
```


## IOC 컨테이너, DI 컨테이너(Inversion Of Control Container)
- 위에서 프로토콜을 통해 의존성을 분리하고 제어의 역전을 이뤄냈습니다. 그러나 매번 프로토콜을 정의하고 관리하는 것은 번거롭고 쉽지않은 작업입니다.
- [커스텀 IOC 컨테이너 구현](https://medium.com/sahibinden-technology/dependency-injection-container-in-swift-89392a309532)을 통해 의존성 주입 및 분리를 관리하는 객체를 만드는 방법도 있으나, 프레임워크 수준에서 관리하는 `IOC 컨테이너`라는 것이 존재합니다.
	- iOS에는 Factory, Resolver 등의 IOC 컨테이너 프레임워크들이 있습니다.
- 프레임워크 내 구현 코드가 프로그래머가 구현해놓은 코드를 호출함으로써 자동으로 의존성 분리 및 주입을 관리합니다.(일반적인 프로그래머 코드 -> 내부 구현 코드 호출 방식에서 제어의 역전)

## DI, IOC의 효능
- 러닝 커브가 굉장히 높지만 사용하는 목적은..
	- 재사용성이 높고 코드를 단순화시킵니다.
	- 결합도의 감소로 각 모듈 별 독립된 테스트가 가능합니다.
	- 개방-폐쇄 원칙을 준수하게 되면서 기능 확장과 동시에 변화를 최소화할 수 있습니다.

## 참고 문헌
- [Clint Jang - DI란?](https://medium.com/@jang.wangsu/di-dependency-injection-%EC%9D%B4%EB%9E%80-1b12fdefec4f)
- [Clint Jang - IOC란?](https://medium.com/@jang.wangsu/di-inversion-of-control-container-%EB%9E%80-12ecd70ac7ea)
- [쏘카 김상우님 - DI와 Swinject](https://velog.io/@heyksw/Swift-DI-%EC%99%80-Swinject)
- [DI 예제](https://medium.com/sahibinden-technology/dependency-injection-in-swift-11756a07a064)
- [IOC 컨테이너 정의 예제](https://medium.com/sahibinden-technology/dependency-injection-container-in-swift-89392a309532)
- [채널톡 제이온님 - 의존 역전 원칙(DIP)이란?](https://steady-coding.tistory.com/388)

