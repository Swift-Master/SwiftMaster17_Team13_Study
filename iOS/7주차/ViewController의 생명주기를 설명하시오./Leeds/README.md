# ViewController의 생명주기를 설명하시오.

<br>

### ViewController?

- iOS 앱에서 화면을 관리하는 객체
- 사용자 인터페이스와 상호작용하며 앱의 비즈니스 로직과 데이터 처리 담당 역할
- viewDidLoad(), viewWillAppear(), viewDidAppear(), viewWillDisappear(), viewDidDisappear()
총 5가지의 메소드가 존재함

<br>

---

<br>

### **ViewController의 생명 주기를 알아봅시다 !**

<br>

**[viewDidLoad](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621495-viewdidload) (view가 load됨)**

- viewController가 메모리에 로드된 다음 호출된다. (처음 만들어질 때 한번만 실행함)
- view 계층 구조 설정, 데이터 로드, 초기화 작업을 수행하는 작업을 한다.
    
    ex) 데이터 소스의 로드나 레이아웃
    

**[viewWillAppear](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621510-viewwillappear) (view가 나타날 것임)**

- 화면에 나타나기 전에 수행해야 하는 작업을 한다.
- 나타나기 직전이므로 유저가 아직 화면을 볼 수 없다
    
    ex) 네트워크 요청 보내기, UI요소를 업데이트 하여 최신 데이터 표시
    

**[viewDidAppear](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621423-viewdidappear) (view가 나타남)**

- 화면에 나타난 후에 수행해야 하는 작업을 한다.
    
    ex) 애니메이션 실행, 음악 재생, 사용자 데이터 수집
    

**[viewWillDisappear](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621485-viewwilldisappear) (view가 사라질 것임)**

- 화면이 사라지기 전에 수행해야 하는 작업을 한다.
    
    ex) UITextField 등 값을 입력받고 있는 데이터 저장
    

**[viewDidDisappear](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621477-viewdiddisappear) (view가 사라짐)**

- 화면이 사라진 후에 수행해야 하는 작업을 한다.
    
    ex) view와 관련된 자원을 해제하고 메모리 관리, 애니메이션 종료
    
 <br>   

생명주기를 그림으로 정리해보자면 이런식으로 정리 할 수 있다..!

![Untitled](%5BSwift%5D%20ViewController%207615418f2cfd4719a8bda92b2247c47e/Untitled.png)

[원본 이미지](https://developer.apple.com/documentation/uikit/uiviewcontroller)

<br>

---

<br>

### **☝ push - pop VC life-cycle 메소드 출력 !**

<br>

push: 뷰가 가로방향으로 전환되고 스택에 뷰를 쌓는 형태로 화면을 업데이트 한다 
(왼쪽에서 오른쪽 or 오른쪽에서 왼쪽)

pop: 스택에 쌓인 뷰 1개를 pop하고 전환된다 (push의 반대 방향으로)

<br>

navigation controller를 활용한 push - pop 예시

- `A View`, `B View` 라는 이름을 가진 2개의 view가 존재

<br>

### 앱 실행시 화면 (아무것도 안건듬)

![스크린샷 2023-03-03 오전 1.10.37.png](%5BSwift%5D%20ViewController%207615418f2cfd4719a8bda92b2247c47e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-03_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.10.37.png)

### PUSH 버튼을 눌렀을 때 ( ==== B 뷰 시작 ==== 부분부터 )

![스크린샷 2023-03-03 오전 1.12.00.png](%5BSwift%5D%20ViewController%207615418f2cfd4719a8bda92b2247c47e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-03_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.12.00.png)

### 위 화면에서 navigation의 Back을 눌렀을 때 ( — 다시 pop을 해보겠음 — 부분부터)

![스크린샷 2023-03-03 오전 1.13.22.png](%5BSwift%5D%20ViewController%207615418f2cfd4719a8bda92b2247c47e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-03_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.13.22.png)

<br>

이런식으로 작동한다 🙂..

<br>

---

<br>

### **✌ VC Life-Cycle 실사용 예시**

<br>

코드로 navigation bar를 없앨때는 두 개의 메소드 `viewDidLoad`, `viewWillappear` 중 어디 메소드에서 사용 해야될까?

일단 코드로 navigation bar를 없앨때는 `self.navigationController?.isNavigationBarHidden = true` **을** 사용한다.

<br>

근데 이것을 `viewDidLoad`에서 작업을 하게 되면 처음에는 navigation bar가 없어져 보일 수 있지만, 

화면 전환 이후 다시 돌아오면 `viewDidLoad`는 다시 불리지 않아서 navigation bar가 보이게 된다.

<br>

따라서 사용자에게 화면이 나타날 때 마다 실행되었으면 하는 작업이 있다면?

`viewWillAppear`메소드에서 사용하면 된다. 

<br>

반대로 사용자에게 화면이 사라질 때 마다 실행되었으면 하는 작업은?

`viewWillDisappear`메소드에서 실행하면 된다.

<br><br><br><br>

### 참고

---

[https://zeddios.tistory.com/43](https://zeddios.tistory.com/43) (5가지 메서드 한줄 정의)

[https://zeddios.tistory.com/44](https://zeddios.tistory.com/44) (push-pop life cycle 상세설명 // A view > B view → B view > A view로 되돌아왔을 때 viewDidLoad가 아닌 viewWillAppear이 출력되는지(?)..)

[https://sueaty.tistory.com/178](https://sueaty.tistory.com/178) (viewDidLoad VS viewWillappear)
