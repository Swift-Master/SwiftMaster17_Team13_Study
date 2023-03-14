# iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?

Q. iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?

A. **UIKit**

---

### **UIKit ?**

- `제스처 처리`, `애니메이션`, `그림 그리기`, `이미지 처리`, `텍스트 처리` 등 사용자 이벤트 처리를 위한 클래스를 포함
- `Table view`, `Slider`, `Button`, `Text field`, `Alert` 등 애플리케이션의 화면을 구성하는 요소 포함
- iOS와 tvOS 플랫폼에서 사용

<br>

그렇기 때문에 자주 사용하는 `UIViewController`, `UIView`, `UIImage` 등 앞에 `UI`가 붙는 클래스들을 사용하려면 <U>**반드시 `UIKit`을 import 해야한다.**</U>

<br>

### 기능별 요소

**User Interface**

- View and Control: 화면에 콘텐츠 표시
- View Controller: 사용자 인터페이스 관리
- Animation and Haptics: 애니메이션과 햅틱을 통한 피드백 제공
- Window and Screen: 뷰 계층을 위한 윈도우 제공

<br>

**User Action**

- Touch, Press, Gesture: 제스처 인식기를 통한 이벤트 처리 로직
- Drag and Drop: 화면 위에서 드래그 앤 드롭 기능
- Pencil Action : 애플 펜슬의 더블 탭 동작
- Focus Animation : 원격 또는 게임 컨트롤러 사용
- Peek and Pop : 3D 터치에 대응한 미리 보기 기능
- Keyboard and Menu: 키보드 입력을 처리 및 사용자 정의 메뉴 표시
