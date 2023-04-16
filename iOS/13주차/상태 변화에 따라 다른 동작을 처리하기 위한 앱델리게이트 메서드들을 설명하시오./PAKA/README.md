<style>
h2 {
  color: orange;
}

h3 {
	color : green;
}
</style>

# 상태 변화에 따라 다른 동작을 처리하기 위한 앱델리게이트 메서드들을 설명하시오.

## 앱 델리게이트란
### 특징 
앱 델리게이트 클래스는 앱의 공유 동작을 관리합니다. 앱 델리게이트는 사실상 앱의 루트 개체이며 UIApplication과 함께 작동하여 시스템과의 일부 상호 작용을 관리합니다. 이를 위해 UIKit 프레임워크는 앱 실행 초기에 UIApplication 객체와 함께 appDelegate 객체를 생성합니다.

### 역할

- 앱의 중앙 데이터 구조 초기화

- 앱의 각 scene에 대한 설정

- 메모리 부족 경고, 다운로드 완료 알림 등 앱 외부에서 발생하는 알림에 응답

- 앱의 scene, view, viewController에 특정된 이벤트가 아닌 앱 그 자체를 대상으로 하는 이벤트에 응답

- Apple 푸시 알림 서비스와 같은 시작 시 필요한 서비스에 등록

</br>

<h2><font color="orange">라이프 사이클 관리</font></h2>
iOS 12 및 이전 버전에서는 UIApplicationDelegate 객체를 사용하여 앱의 주요 수명 주기 이벤트를 관리했습니다. 특히 appDelegate 클래스의 메서드를 사용하여 앱이 foreground에 진입하거나 background으로 이동할 때 앱의 상태를 업데이트합니다.

</br>
<h3><font color = "green">앱이 실행될 때 호출되는 메서드들</font></h3>
</br>
<details><summary>간략</summary>

![앱 실행 단계](https://user-images.githubusercontent.com/116094622/232279109-7e6509f6-a670-4daf-b747-a6a7c6158dc1.png)

</details>
</br>
<details><summary>상세</summary>

앱 실행시 사이클 상세
![상세](https://user-images.githubusercontent.com/116094622/232280168-9ea4a694-dd7e-48ad-b293-1ab31a721c24.png)

앱 상태 저장 과정 상세
![보존 상세](https://user-images.githubusercontent.com/116094622/232283721-50112549-d02e-4da4-89b7-8994522ecd60.png)

</details>

</br>
시스템은 사용자가 홈 화면에서 앱의 아이콘을 tap했을 때 실행시킵니다. 또는 앱의 특정 이벤트 요청에도 해당 이벤트를 처리하기 위해 background에서 앱을 실행시키기도 합니다. 
</br>
실행과정은 크게 UIApplication, appDelegate 객체 생성 - 최초 초기화 - 이전 상태 복원 & 현 상태 저장 - 최종 초기화로 이뤄지게 됩니다.</br>
<font color="red"> scene 기반 앱에서도 app delegate를 사용하여 앱 실행과 삭제 같은 기초 이벤트들을 관리합니다.</font>

</br>

<details><summary>application(_:willFinishiLaunchingWithOptions:), application(_:didFinishLaunchingWithOptions:)</summary>

- 호출 시점 : UIKit이 앱의 실행 사이클 시작점에서 호출합니다.
- 동작 효과 
	- 앱의 기본 데이터 구조 초기화
	- 앱에서 실행해야 될 리소스가 있는지 검사
	- 앱의 최초 실행시 하는 일회성 설정 작업을 수행
	- 앱이 사용하는 중요 서비스에 연결(ex:애플 push 알림 서비스)
	- 앱이 설치되는 이유 정보를 위한 실행 옵션 dictionary 체크(**<font color="red">scene기반 앱의 경우 `application(_:configurationForConnecting:options:)`에서 검사</font>**)
	- 사용자가 마지막으로 앱을 사용했을 때 수행한 작업 반영(application(_:didFinishLaunchingWithOptions:)에서 수행, **<font color="red">scene기반 앱이 아닌경우만 가능</font>**)

</details>
</br>
<details><summary>application(_:shouldRestoreApplicationState:)와 application(_:shouldSaveApplicationState:), application(_:shouldRestoreSecureApplicationState:)와 application(_:shouldSaveSecureApplicationState:)</summary>

- 호출 시점 : 최초 앱 초기화가 일어나는 중간에 호출됩니다.
- 동작 효과 : 만약 앱이 state restore을 지원한다면 true를 반환하여 Restore UI State 작업을 수행하게 됩니다.
- **<font color = "red"> 특이사항 :  application(_:shouldRestoreApplicationState:)와 application(_:shouldSaveApplicationState:) 메서드는 ios 13.2 버전 이후로 deprecated되었습니다. 이후 버전에서는 application(_:shouldRestoreSecureApplicationState:), application(_:shouldSaveSecureApplicationState:)을 사용합니다.</font>**

</details>
</br>
<details><summary>application(_:viewControllerWithRestorationIdentifierPath:coder:)</summary>

- 호출 시점 : UI State를 복구하는 도중 UIKit은 보존된 인터페이스 내에서 뷰 컨트롤러 객체를 생성하거나 찾으려고 시도합니다. 
	- 처음에는 viewController(withRestorationIdentifierPath:coder:)를 호출하여 복구 대상 뷰컨트롤러의 restoreration class가 있는지 묻습니다. 만약 없다면, 추가 탐색을 계속합니다.
	- 두번째로는 appDelegate의 application(_:viewControllerWithRestorationIdentifierPath:coder:)을 호출하여 restoreration class가 있는지 묻습니다. 만약 여기에도 없다면 같은 path를 가진 동일 객체가 존재하는지 찾고, 그마저 없다면 스토리보드의 뷰컨트롤러를 가져와서 생성합니다.

- 동작 효과 : UIKit이 요구하는 뷰컨트롤러 객체를 생성하거나 이미 존재하는 뷰컨트롤러를 반환합니다. 만약 appDelegate가 제공하는 뷰컨트롤러가 없다면 nil을 반환합니다.

</details>
</br>
<details><summary>application(_:willEncodeRestorableStateWith:)</summary>

- 호출 시점 : application(_:shouldSaveApplicationState:) 또는 application(_:shouldSaveSecureApplicationState:)의 return 값이 true라면  앱 상태 저장 사이클이 시작되고 그 사이클 초기에 호출됩니다.

- 동작 효과 : 앱의 설정 상태 또는 UI와 관련된 다른 정보들을 저장합니다. **<font color="red">데이터 보존 process는 실제 DB나 icloud처럼 데이터의 영구 저장을 보장하지 않습니다.</font>**
</details>

</br>

<h3><font color = "green">foreground로 전환시 호출되는 메서드들</font></h3>
</br>

<details><summary>applicationWillEnterForeground(_:) `sceneWillEnterForeground(_:)`</summary>
- 호출 시점 : 시스템은 foreground로 이동하기 전 앱을 `inactive` 상태로 만듭니다. 이를 위해 UIKit이background에 존재하는 앱을 inactive상태로 전환시킬 때  호출됩니다.

- 동작 효과 : background에서 foreground로 전환될 때 디스크로부터 리소스들을 불러오고 네트워크로부터 데이터를 가져옵니다.

</details>
</br>

<details><summary>applicationDidBecomeActive(_:) `sceneDidBecomeActive(_:)`</summary>

- 호출 시점 : 시스템은 UI를 화면에 표시하기 전에 앱을 `active` 상태로 전환시킵니다. 이 과정에서 두 메서드는 앱의 UI와 런타임 동작을 설정하기 위해 호출됩니다.

- 동작 효과 : 앱의 UI와 런타임 동작을 설정할 수 있는 코드가 위 메서드에 포함되어 있습니다. 구체적으로는...
	- 필요하다면 현재 보여지는 뷰 컨트롤러를 바꿉니다.
	- 뷰와 컨트롤 객체(버튼, 슬라이더 등..)의 상태 및 데이터를 업데이트 합니다.
	- 일시중지된 게임을 재개할 수 있는 컨트롤 객체를 화면에 표시합니다.
	- 작업을 실행하기 위해 사용할 디스패치 큐를 시작시키거나 재개시킵니다.
	- 데이터 소스 객체를 업데이트합니다.
	- 주기적 작업의 타이머를 시작시킵니다.

</details>
</br>

<h3><font color = "green">background로 전환시 실행되는 메서드들</font></h3>

<details><summary>applicationWillResignActive(_:) `sceneWillResignActive(_:)`</summary>

- 호출 시점 : 시스템은 foreground 앱을 종료하거나 시스템 알림이 표시와 같은 여러가지 이유로 앱을 비활성화시킵니다. 비활성화 과정에서 UIKit이 메서드를 호출하게 됩니다.

- 동작 효과 : 사용자의 데이터를 보존하고 대부분의 주요 작업들을 중단시켜 앱의 동작을 최소화합니다.
	- 디스크에 데이터 저장 및 열린 파일 닫기
	- 디스패치, NSOperation 대기열 일시중지
	- 새롭게 실행될 작업 스케줄링 중단
	- 활성화된 타이머 모두 비활성
	- 자동으로 게임 플레이 일시중지
	- Metal 프레임워크, OpenGL 등의 그래픽 작업 중단

</details>
</br>
<details><summary>applicationDidEnterBackground(_:) `sceneDidEnterBackground(_:)`</summary>

- 호출 시점 : applicationWillResignActive(_:)을 통해 active 상태에서 In-active 상태로 전환된 앱이foreground에서 background로 이동하게 되는 시점에 호출됩니다.

- 동작 효과 : foreground에서 점유하고 있던 시스템 메모리를 해제하고 모든 공유 리소스를 해제시킵니다.
	- 직접 파일에서 읽어오고 있는 이미지나 미디어를 제거합니다.
	- 디스크에서 재생성 또는 다시 호출가능한 메모리 점유율이 높은 객체들을 제거합니다.
	- 카메라 및 다른 공유 하드웨어 리소스와의 연결을 해제합니다.
	- UI에서 민감한 개인정보를 숨깁니다.
	- 알림이나 다른 일시적 인터페이스를 제거합니다.
	- 공유 시스템 데이터베이스와의 연결을 닫습니다.
	- Bonjour 서비스 등록을 해제하고 연관 소켓 추적을 중단합니다.
	- 그래픽 작업(Metal framework, OpenGL)이 끝났는지 확인합니다.

</details>
</br>

<h3><font color = "green">앱이 종료될때 실행되는 메서드들</font></h3>

</br>

<details><summary>applicationWillTerminate(_:)</summary>

- 호출 시점 : 사용자가 앱을 종료했을 때 해당 앱이 background 상태를 미지원 또는 ios 3 버전 미만이거나 메모리 관리 등의 이유로 시스템이 background에 존재하는 앱을 제거해야 할 때 호출됩니다.

- 동작 효과 : 앱이 곧 종료되며 메모리에서 완전히 제거됨을 notification을 통해 앱에 알립니다. 공유 리소스 해제, 사용자 데이터 저장 및 타이머 무효화 등의 background 전환에서 했던 것과 유사한 앱 동작 최소화 과정을 수행합니다. 소요시간은 약 5초이며, 5초 이후에도 메서드가 반환되지 않으면 시스템이 자동으로 모든 프로세스를 중단시킵니다.

</details>
</br>
<details><summary>application(_:didDiscardSceneSessions:)</summary>

- 호출 시점
	- 사용자가 app Switcher에서 scene을 제거하여 UIKit이 Scene에 연결된 세션을 모두 없애기 전에 호출
	- 화면에 표시할 수 없는 scene이 존재하는 경우 UIKit이 호출하며, 메모리 관리를 위해 background의 scene을 삭제하는 경우에는 호출되지 않습니다.(scene은 삭제되나 연관 Session들은 삭제되지 않습니다)
	- 앱이 현재 not running 상태라면, 앱 실행 이후 이 메서드를 호출합니다.

- 동작 효과 : 앱의 데이터 구조를 업데이트하고 scene과 연관된 모든 리소스를 해제합니다.

</details>
</br>
<details><summary>
<h2><font color = "orange">life cycle 이외의 이벤트 관리(그냥 참고용)</font></h2></summary>

<h3><font color = "green">메모리 경고시 사용되는 메서드</font></h3>
</br>

#### applicationDidReceiveMemoryWarning(_:)
- 호출 시점 : 시스템이 가용 메모리가 부족하고 suspended 앱을 종료해도 메모리를 회수 불가능할 때 UIKit이 실행중인 앱에 메모리 경고를 보내면서 호출합니다.
- 동작 효과 : 재생성 가능하거나 디스크에서 다시 호출가능한 cache 데이터를 제거하여 가능한 한 많은 메모리를 확보합니다. UIViewController 클래스의 didReceiveMemoryWarning() 및 didReceiveMemoryWarningNotification 알림과 함께 사용하여 앱 전체에서 메모리를 해제합니다.

<h3><font color = "green">사용자가 기기를 lock/unlock시 호출되는 메서드</font></h3>
</br>

#### applicationProtectedDataDidBecomeAvailable(_:), applicationProtectedDataWillBecomeUnavailable(_:)
- 동작 효과 : 현재 기기가 잠금 상태가 아닐 때에만 보호된 파일에 접근이 가능하도록 합니다. 접근 자체가 문제 없더라도 잠금 상태에서 해당 파일에 접근이 있다면, 그 접근을 해제시킵니다.

<h3><font color = "green">Handoff(아이폰-맥북-아이패드 간 클립보드 공유 같은 멀티플랫폼 작업)시 호출되는 메서드</font></h3>
</br>

#### application(_:didUpdate:)

<h3><font color = "green">시간 변화시 호출되는 메서드</font></h3>
</br>

####  applicationSignificantTimeChange(_:)

<h3><font color = "green">open URL 리소스 접근시 호출되는 메서드</font></h3>
</br>

#### application(_:open:options:)

</details>

## 참고 문헌

- [UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)
- [앱의 launch 사이클](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app/about_the_app_launch_sequence)
- [앱의 restoration 사이클](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches/about_the_ui_restoration_process)
- [앱의 데이터 보존 사이클](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches/about_the_ui_preservation_process)