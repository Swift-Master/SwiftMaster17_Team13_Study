# 앱 화면의 콘텐츠를 표시하는 로직과 관리를 담당하는 객체를 무엇이라고 하는가?

`An object that manages a view hierarchy for your UIKit app.`  
애플 공식 문서에서 **UIViewController**를 UIKit 앱에서 뷰 계층을 관리하는 객체라고 설명합니다.  
따라서 이 질문의 정답이 되겠습니다 ^-^!

UIViewController 클래스는 모든 뷰 컨트롤러에 공통적으로 공유되는 동작들을 정의합니다.  
일반적으로 UIViewController 클래스의 인스턴스를 직접 만들지 않고,  
UIViewController를 상속해서 뷰 계층을 관리하는데에 필요한 메서드와 프로퍼티를 추가해야 합니다.

ViewController의 주요 동작으로는,
- 일반적으로 데이터의 변화로 인한 화면 컨텐츠 업데이트
- 앱의 화면과 사용자 간의 상호작용
- 뷰 크기 조정과 전체 인터페이스의 레이아웃 관리
- 다른 객체들과의 상호작용

### 출처
[UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)