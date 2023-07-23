# RIBS 아키텍처
## 출처
[Uber/RIBS ios 공식 튜토리얼](https://github.com/uber/RIBs/wiki)
## 나오게 된 배경
### 안드로이드와의 협업
ios와 안드로이드에서 동일한 아키텍처를 채택함으로써 협업과 생산성 향상에 기여합니다.
### 전역 상태 최소화
전역 상태 값(ex: 싱글톤)의 원치 않는 변화 발생을 분리된 계층 구조와 캡슐화를 통해 예방
### SOLID 개방-폐쇄 원칙 준수를 통한 테스트 용이함과 계층 별 균등한 책임
DI 트리 구조와 RX, 프로토콜을 통해 Router, Interactor, Builder 등 각 계층의 독립성 보장
### 비즈니스 로직 중심
기존 VIPER 아키텍처는 앱의 상태가 View에 주도하에 결정되기 때문에 비즈니스 로직만 분리하는 것이 어려움.

## 구조

<p align = middle><image src = "https://github.com/uber/ribs/raw/assets/documentation/ribs.png"></image></p>

### Router

- 역할 : Interactor를 수신하고 하위 RIB를 부착/탈착합니다.
- 내부 요소
    - Interactable 프로토콜 : Interactor 구현체(Class)가 준수하는 프로토콜로 Interactor-Router간 통신이 가능하게 합니다.
    - ViewControllerable 프로토콜 : View/ViewController 가 준수하는 프로토콜로 ViewController-Router간 통신이 가능하게 합니다.
    - Router 구현체(Class)
        - 생성시 다른 RIB들의 Builder객체들을 외부에서 주입받고 Interactor의 Router 속성에 자기자신을 할당함으로써 ViewController-Router를 서로 연결합니다.(delegate 패턴과 유사)
        - Interactor의 Routing 프로토콜을 준수하여 다른 RIB로 전환하는 로직이 구현되어 있습니다. 화면 전환 시 `StoryBoard.instantiateViewController`를 통해 다음 ViewController 객체를 가져오는 것처럼, 다른 RIB의 Builder객체의 `build()`함수를 통해 RIB를 생성하고 탈/부착합니다. 

### Interactor

- 역할
    - 비즈니스 로직(RX 구독-상태변경 로직,데이터 저장위치 결정, RIB연결 결정)이 포함되어 있는 계층
    - 상태관리 메서드 `willResignActive()`와 RX 메서드`dispose()`를 통해 Interactor가 존재할 때만 비즈니스 로직이 적용되도록 제한합니다. 이를 통해 전역상태 최소화의 장점을 가져갈 수 있습니다.
- 내부 요소
    - ViewableRouting 프로토콜 : 다른 RIB의 탈/부착용 메서드들이 선언되어 있고 Router에서 채택하여 라우팅 로직을 구현합니다.
    - Presentable 프로토콜 : Interactor-Presenter간 통신을 위한 프로토콜로 PresentableListener 프로토콜타입 변수가 기본적으로 선언되어 있습니다. Presenter 구현체가 프로토콜을 채택하여 내부 메서드를 구현하게 됩니다.
    - Listener 프로토콜 : AnyObject 상속 프로토콜로 상위 RIB Router의 Interactable 프로토콜이 상속받게 됩니다. 자손 RIB의 Builder는 Interactor를 생성 후 Listener에 상위 RIB의 Interactable 객체(Interactor클래스)를 주입합니다.
    - Interactor 클래스
        - Router와 통신을 위한 Routing프로토콜 타입 변수, 부모 RIB와 통신을 위한 Listener프로토콜 타입 변수, 자손 RIB Interactor의 메서드가 구현되어 있습니다.
        - 생성시 주입받은 presenter의 PresentableListener(위 Listener와 다름)에 자기자신을 할당함으로써 Interactor-Presenter를 서로 연결합니다.

### Builder

- 역할
    - RIB 구성 클래스 및 각 자손들의 Builder를 인스턴스화
    - DI 매커니즘이 활용되며 클래스 생성로직을 분리한다면 테스트를 위해 Mocking이 가능하다.
- 내부 요소
    - Dependency 프로토콜 : 외부에서 주입받는 종속성들이 포함되어 있습니다.
    - Component 클래스 : 종속성 관리 클래스로 Dependency 프로토콜의 종속성들은 `fileprivate`으로 외부 접근을 제한하고 현재 RIB에서 사용하지 않는 종속성은 extension에 포함시킵니다. 부모 RIB의 Component는 자손 RIB의 빌더에 주입되어 자손이 부모의 종속성에 접근하는 것을 가능하게 만듭니다.
    - Buildable 프로토콜 : Builder클래스가 준수하는 프로토콜로 `build()`메서드가 선언되어 있습니다.
    - Builder 클래스 : 생성 시 dependency를 주입받아 `build()`메서드를 통해 RIB 각 요소들을 생성 후 Router로 제어를 넘깁니다. 자손 RIB라면 이때 부모의 Interactor와 Interactor를 연결합니다.(listener에 주입받은 interactor 할당)

### Presenter, ViewController
- presenter 역할 : Presenter는 Interactor와 View/ViewController 사이에서 Model - ViewModel 변환을 담당합니다. 그러나 역할이 워낙 작기 때문에 ViewController나 Interactor가 대신하고 생성하지 않는 경우도 있습니다.(튜토리얼에서는 ViewController가 대신 수행)
- ViewController는 UI 관련 로직만 수행합니다.
- 구성요소
    - PresentableListener 프로토콜 : Interactor 구현체가 준수하는 프로토콜로 Interactor가 프로토콜 메서드를 구현하고 생성시 Presenter 클래스의 PresentableListener 타입의 Listener 변수에 자기자신을 할당함으로써 Interactor-Presenter를 연결합니다.
    - Presenter 구현체(Class) : 채택한 Router의 프로토콜 Presentable과 ViewControllerable의 상세 구현과 PresentableListener 변수가 존재합니다.

## 로직 예시

로그인 화면에서 사용자가 로그인 버튼을 눌렀을 때 다음 화면으로 전환하고자 합니다.

이 앱의 RIB 구조는 다음과 같습니다.

<p align = middle><image src = "https://github.com/uber/ribs/raw/assets/tutorial_assets/ios/tutorial2-composing-ribs/project-structure.png" width = 85%></image></p>

현재 로그인 화면은 LoggedOut RIB이고 로그인 버튼 클릭시 Root RIB -> LoggedIn -> OffGame RIB로 전환됩니다.

LoggedIn RIB는 Viewless RIB로 Root의 ViewController를 주입받습니다.

### 과정
1. 사용자가 로그인 버튼을 누르면 LoggedOut RIB ViewController의 함수를 호출하고, 다시 함수 내부에서 ViewController의 listener의 `login` 메서드를 호출합니다.
```swift
protocol LoggedOutPresentableListener: AnyObject {
    func login(withPlayer1Name: String?, player2Name: String?)
}

final class LoggedOutViewController: UIViewController, LoggedOutPresentable, LoggedOutViewControllable {

    weak var listener: LoggedOutPresentableListener?

    private func buildLoginButton(withPlayer1Field player1Field: UITextField, player2Field: UITextField) {
        let loginButton = UIButton()

        //LoggedOutViewController -> LoggedOutInteractor
        loginButton.rx.tap
            .subscribe(onNext: { [weak self] in
                self?.listener?.login(withPlayer1Name: player1Field.text, player2Name: player2Field.text)
            })
            .disposed(by: disposeBag)
    }
}
```
2.  listener는 LoggedOutInteractor 클래스 생성 시점에 LoggedOutInteractor를 주입받았기 때문에 LoggedoutInteractor로 제어권이 넘어갑니다.

```swift

protocol LoggedOutListener: AnyObject {
    func didLogin(withPlayer1Name player1Name: String, player2Name: String)
}

final class LoggedOutInteractor: PresentableInteractor<LoggedOutPresentable>, LoggedOutInteractable, LoggedOutPresentableListener {

    weak var listener: LoggedOutListener?

    override init(presenter: LoggedOutPresentable) {
        super.init(presenter: presenter)
        presenter.listener = self 
        // ViewController의 리스너에 자기자신을 주입
    }

    func login(withPlayer1Name player1Name: String?, player2Name: String?) {

        // LoggedOutInteractor -> RootInteractor
        listener?.didLogin(withPlayer1Name: player1NameWithDefault, player2Name: player2NameWithDefault)
    }
}

```

3. 호출된 LoggedOutInteractor의 `login()`메서드는 내부에서 listener의 `didLogin()`메서드를 호출합니다. 이때 listener는 LoggedOutBuilder의 `build()`메서드에서 부모의 Interactor인 RootInteractor를 주입받았습니다.
```swift
final class RootRouter: LaunchRouter<RootInteractable, RootViewControllable>, RootRouting {

    private let loggedOutBuilder: LoggedOutBuildable

    // LoggedOutRIB로 전환하는 메서드. build 메서드 호출시 매개변수로 RootInteractor를 프로토콜 타입으로 전달합니다.
    private func routeToLoggedOut() { 
        let loggedOut = loggedOutBuilder.build(withListener: interactor)
        // ... 중략
    }
}

final class LoggedOutBuilder: Builder<LoggedOutDependency>, LoggedOutBuildable {
    // listener 매개변수에 전달받은 RootInteractor를 LoggedInteractor의 listener에 할당합니다. 
    func build(withListener listener: LoggedOutListener) -> LoggedOutRouting {
        _ = LoggedOutComponent(dependency: dependency)
        let viewController = LoggedOutViewController()
        let interactor = LoggedOutInteractor(presenter: viewController)
        interactor.listener = listener
        return LoggedOutRouter(interactor: interactor, viewController: viewController)
    }
}
```

4. 호출된 RootInteractor의 `didLogin()` 메서드 내부에서 router 변수의 routeToLoggeIn 메서드를 호출합니다. RootRouter 생성 시점에 router 변수에 RootRouter를 할당했기 때문에 결과적으로 RootRouter로 제어권이 넘어갑니다.

```swift
final class RootRouter: LaunchRouter<RootInteractable, RootViewControllable>, RootRouting {
    // 생성 시점에 자기자신을 RootInteractor의 router 변수에 할당합니다.
    init(interactor: RootInteractable,
         viewController: RootViewControllable,
         loggedOutBuilder: LoggedOutBuildable,
         loggedInBuilder: LoggedInBuildable) {
        super.init(interactor: interactor, viewController: viewController)
        interactor.router = self
    }
    // loggeOut RIB를 탈착하고 loggedIn RIB를 부착합니다.
    func routeToLoggedIn(withPlayer1Name player1Name: String, player2Name: String) {

        if let loggedOut = self.loggedOut {
            detachChild(loggedOut)
            viewController.dismiss(viewController: loggedOut.viewControllable)
            self.loggedOut = nil
        }

        let loggedIn = loggedInBuilder.build(withListener: interactor,player1Name: player1Name,player2Name: player2Name)
        attachChild(loggedIn)
    }
}

protocol RootRouting: ViewableRouting {
    func routeToLoggedIn(withPlayer1Name player1Name: String, player2Name: String)
}

final class RootInteractor: PresentableInteractor<RootPresentable>, RootInteractable, RootPresentableListener {

    weak var router: RootRouting?
    // RootInterActor -> RootRouter
    func didLogin(withPlayer1Name player1Name: String, player2Name: String) {
        router?.routeToLoggedIn(withPlayer1Name: player1Name, player2Name: player2Name)
    }
}
```

5. 호출된 RootRouter의 `routeToLoggedIn`메서드는 현재 RIB계층구조에서 현재 부착된 LoggedOutRIB를 탈착하고 LoggedInRIB를 부착합니다. `attachChild`메서드는 부착시키는 RIB의 Interactor를 활성화시키고 Router의 load 함수를 호출합니다.

6. 호출된 LoggedInRouter의 `load(didload)`메서드는 OffGameBuilder의 `build` 메서드를 호출하여 OffGameRIB를 생성하고 RIB 계층구조에 부착합니다. LoggedInRIB는 부모RIB이므로 이번에는 탈착하지 않습니다. 최종적으로 OffGameViewController 화면으로 전환되게 됩니다.

```swift

final class LoggedInRouter: Router<LoggedInInteractable>, LoggedInRouting {

    override func didLoad() {
        super.didLoad()
        attachOffGame()
    }

    private let viewController: LoggedInViewControllable
    private let offGameBuilder: OffGameBuildable
    private var currentChild: ViewableRouting?

    private func attachOffGame() {
        let offGame = offGameBuilder.build(withListener: interactor)
        self.currentChild = offGame
        attachChild(offGame)
        viewController.present(viewController: offGame.viewControllable)
    }
}
```



