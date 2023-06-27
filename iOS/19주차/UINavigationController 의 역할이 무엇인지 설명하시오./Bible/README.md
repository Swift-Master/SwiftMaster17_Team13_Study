# UINavigationController의 역할이 무엇인지 설명하시오.

먼저, 컨테이너 뷰 컨트롤러는 여러 개의 자식 뷰 컨트롤러를 계층적으로 관리하하는 유형입니다.  
기본적으로 앱의 데이터를 표시하는 것을 **컨텐츠 뷰 컨트롤러(content view controller)**라고 하고,  
**컨테이너 뷰 컨트롤러(container view controller)**는 다른 뷰 컨트롤러를 화면에 표시하고 정렬하는 탐색 경험을 제공합니다.  
(UINavigationController, UITabBarController, UIPageViewController 등등)

UINavigationController는 계층적으로 콘텐츠를 탐색하기 위한 스택 기반의 컨테이너 뷰 컨트롤러입니다.

하나 이상의 자식 뷰 컨트롤러를 관리하며 한 번에 하나의 자식 뷰 컨트롤러를 화면에 보여줍니다.  
뷰 컨트롤러의 항목을 선택하면 새로운 뷰 컨트롤러를 애니메이션과 함께 화면에 푸시하고 이전 화면은 숨김 처리되어 뒤로가기 버튼을 통해 이전 뷰 컨트롤러를 다시 화면에 보이게 할 수 있습니다.

앱이 계층적인 데이터 구성을 가지고 있다면 UINavigationController 인터페이스로 각 계층에 적합한 화면을 표시할 수 있도록 합니다.

iOS의 설정 앱에서 제공하는 내비게이션 인터페이스를 예로 들어봅시다

![navigation_interface_1](https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/59015538/2cc6471d-e92f-48f5-81ed-07ab9aec47df)

NavigationController는 스택 기반이라고 했는데,  
이 사진에서 맨 왼쪽에 있는 뷰 컨트롤러는 루트 뷰 컨트롤러가 되어 스택의 맨 아래에 위치하게 됩니다.

그리고 맨 오른쪽에 있는 뷰 컨트롤러는 스택의 가장 위에 있는 항목이 됩니다.

상단 내비게이션 바에 뒤로가기(back) 버튼이나 왼쪽 가장자리를 스와이프하는 동작을 통해 스택 위에 있는 뷰 컨트롤러를 제거하면서 이전 화면으로 가게 됩니다.

또한 내비게이션 컨트롤러는 델리게이트 객체와 함께 동작하게 됩니다.  
UINavigationControllerDelegate 프로토콜을 채택한 델리게이트 객체는 뷰 컨트롤러의 push, pop 동작을 오버라이드 하거나 사용자 정의 애니메이션 전환을 제공하고 내비게이션 인터페이스의 방향(기기의 가로 or 세로)을 지정할 수 있습니다.

## 참고
[UINavigationController](https://developer.apple.com/documentation/uikit/view_controllers/creating_a_custom_container_view_controller)