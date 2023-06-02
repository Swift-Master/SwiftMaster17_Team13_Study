# UIApplication 객체의 컨트롤러 역할은 어디에 구현해야 하는가?

## UIApplication

### 생성 

![core app objects](https://docs-assets.developer.apple.com/published/4e7c26b6ad/ff7aa08f-4857-44ce-88d5-7dacbef84509.png)

- 모든 iOS 앱은 시작과 동시에 UIKit이  UIApplicationMain() 함수를 호출하여 단 하나의 UIApplication 싱글톤 공유 인스턴스를 생성하여 사용합니다.(특별한 경우 UIApplication 하위 객체가 존재하기도 함)

### 컨트롤러로서의 역할 구현
#### 작은 의미 : 이벤트를 적절한 객체로 전달하는 중개자

- UIApplication 인스턴스는 입력된 사용자 이벤트의 초기 라우팅을 처리합니다. 제어 객체(UIControl 클래스의 인스턴스)가 전달한 작업 메시지를 적절한 대상 객체로 발송합니다. 

- 가끔 앱이  입력 이벤트를 시스템보다 먼저 처리해야 한다면 UIApplication 클래스의 자손 객체에서 sendEvent()나 sendAction(_:to:from:for:) 메서드를 재정의합니다. 가로채는 모든 이벤트에 대해 해당 메서드에서 이벤트를 처리한 후 `super.sendEvent(event)`을 호출하여 시스템에 다시 전달합니다.
- 이벤트 가로채기는 거의 필요하지 않으며 가능하면 피해야 합니다.🙏🏻

#### 넓은 의미 : 앱의 전체적인 의사결정을 하는 관리자

- UIApplication 클래스는 UIApplicationDelegate 프로토콜을 준수하는 appdelegate를 정의합니다. UIApplication 클래스가 중요한 런타임 이벤트(앱 시작, 메모리 부족 경고 및 앱 종료)를 appdelegate에게 시스템과 앱의 상호작용을 관리하도록 합니다.

## 결론
- 중개자로서의 역할을 의미한다면
	- UIApplication 클래스의 sendEvent(), sendAction() 메서드를 재정의하여 로직 구현
- 의사결정을 담당하는 관리자로서의 역할을 의미한다면
	- appDelegate의 UIApplicationDelegate 프로토콜 메서드에 로직 구현

# 참고문헌
- [공식문서 - UIKit 이용 앱 개발에 대해](https://developer.apple.com/documentation/uikit/about_app_development_with_uikit)
- [공식문서 - UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)