# UIWindow 객체의 역할은 무엇인가?

## 역할
### 주요
#### 앱의 콘텐츠를 표시하는 main window를 제공하고, 추가 콘텐츠를 표시하려면 추가 창(필요에 따라)을 만듭니다.
- 스토리보드를 사용하는 앱은 Xcode Template이 자동으로 제공하는 app delegate 클래스의 window 속성을 main window로서 사용합니다. 그래서 코드로만 UI를 작성하게 될 경우 반드시 window 객체를 만들어서 사용해야 합니다.
- 그 이상도 가능하나 일반적으로 하나의 스크린에는 하나의 window만 사용하고 게임 리모트 플레이나 다른 공유된 화면이 있을 경우 여러 개의 window를 만듭니다.


### ETC
#### 다른 창과 비교하여 창의 가시성에 영향을 주는 창의 z축 수준을 설정합니다.
- windowLevel
	- z축에서의 window의 위치를 나타내는 프로퍼티입니다.
- UIWindow.Level 구조체
	- window간의 레이아웃을 설정할 수 있는 UIWindow의 프로퍼티입니다.
#### 창을 표시하고 키보드 이벤트의 대상으로 만듭니다.
- didBecome`Visible, Hidden, Key, ResignKey` Notification
	- window와 관련된 Notification입니다.
- keyboardWill, Did ~Notification 시리즈
	- 키보드 숨기기, 보이기, 애니메이션 동작이나 위치 변경 같은 키보드와 관련된 Notification을 시스템(정확히는 NSNotificationCenter)으로부터 전달받습니다.
	- 이 작업을 위해 Notification들에 대한 옵저버가 등록되어 있습니다.
- sendEvent()
	- 이벤트들을 적절한 뷰로 전달시킵니다. 
#### window의 좌표계 좌표 값 변환
- convert() 함수들
	- 여러 형태로 오버로딩되어 있습니다. 
		- 좌표형태 : CGRect(x좌표,y좌표,너비,길이)와 CGPoint(x,y좌표)
		- 대상 : 현재 UIWindow인지, 다른 UIWindow인지
	- 두 파라미터를 조합하여 현재 또는 다른 window의 좌표를 조정할 수 있습니다.
#### window의 rootViewController 변경
- rootViewController
	- 모순적으로 window 자체는 실제로 보여지는 형태가 없습니다. 그래서 rootViewController 프로퍼티에 view들을 담아 원하는 화면을 보여주게 됩니다. 
	- 또한 화면과 관련된 로직은 UIViewController에서도 구현가능하기 때문에 우리는 window를 직접 상속받아 사용할 필요가 없습니다.
#### window가 표시되는 화면 변경
- 아래의 프로퍼티 및 함수들은 현재 window를 키(메인) 윈도우로 변경하거나 실제 화면에 보여지도록 설정할 수 있습니다.
- isKeyWindow : 해당 window가 메인 window라면 true, 아니라면 false를 반환합니다.
- canBecomeKey : 메인 window가 될 수 있는 window라면 true, 아니라면 false인 bool값입니다.
- makeKeyAndVisible() : 해당 window를 메인 윈도우로 등록하고 화면에 보여지도록 합니다.
- becomeKey() : 현재 메인 Window를 불러옵니다.
- resignKey() : 현재 메인 window로 등록된 window를 해제합니다.
- makeKey() : 해당 window를 메인 window로 만듭니다.

## 참고 문헌
- [애플 공식 문서-UIWindow](https://developer.apple.com/documentation/uikit/uiwindow)