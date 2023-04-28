# scene delegate에 대해 설명하시오.

**AppDelegate**와 **SceneDelegate**는 iOS 애플리케이션 Life cycle 관리를 담당하는 클래스

<br>

![CDwg5Ny](https://user-images.githubusercontent.com/122095401/230316198-419ac027-58b2-4f35-b1d5-09c9735c22fb.png)
~ iOS 12

<br>

![D4VfgIv](https://user-images.githubusercontent.com/122095401/230316160-111c2441-e62a-4a9a-9c32-0d1924fbc3eb.png)
iOS 13~

<br><br>

### **AppDelegate?**

- iOS 애플리케이션의 진입점(entry point)으로, 앱 실행 중에 발생하는
시스템 이벤트 및 앱 수명주기 이벤트 처리
- 앱 시작, 종료, background/foreground전환, 메모리 부족
- 앱의 상태 변화를 감지하고 앱에 필요한 초기화 작업을 수행
- 사용자 정보와 같은 전역 데이터를 관리하고 앱의 상테를 저장

<br>

### **SceneDelegate?**

- iOS 13 이후 추가된 클래스
- 다중 윈도우 및 다중 화면을 지원 하도록 개발된 앱에서 Scene Session 관리
- 새로운 Scene이 생성 될 때마다 새로운 Scene Session을 생성 및 관리
- Scene의 생명주기 관리 담당
- Scene이 background로 이동할 때, view 및 VC의 상태 저장

<br>

정리하자면,

AppDelegate = 앱의 전반적인 life cycle을 관리

SceneDelegate = 화면에 무엇(scene/window)을 보여줄지 관리

<br>

### **Scene이 무엇일까?**

- UI와 사용자 상호작용을 관리하는 객체
- 사용자가 앱 아이콘을 탭하여 앱을 시작하면 scene이 생성된다.
- 앱에서 다른 기능을 실행할 때마다 새로운 scene이 생성될 수 있다.

<br>

### **SceneDelegate에서는 제공하는 메소드는 어떤게 있을까?**

- **scene(_: willConnectTo: options: )**
    - 새로운 scene이 시스템에 연결되기 직전에 호출
    - 해당 메소드를 사용하여 scene의 구성과 초기 데이터 설정
- **sceneDidDisconnect(_ :)**
    - scene이 시스템에서 분리 되었을 때 호출 (마무리 작업 수행)
    - ex) 앱 내의 데이터 저장, 현재 scene의 상태 저장
- **sceneDidBecomeActive(_ :)**
    - scene이 활성화되어 사용자의 입력을 받을 수 있을 때 호출
    - ex) 사용자가 버튼을 탭하거나 터치, 입력 가능
- **sceneWillResignActive(_ :)**
    - scene이 비활성화되어 사용자의 시각적 입력을 받을 수 없을 때 호출
    - ex) 현재 scene에서 작업 중인 데이터 저장, 실행중인 애니메이션 일시 중지 가능
- **sceneWillEnterForeground(_ :)**
    - scene이 background → foreground로 전환되기 직전에 호출
    - ex) 현재 scene에서 실행중인 애니메이션 다시 시작, 리소스 초기화 작업 수행 가능
- **sceneDidEnterBackground(_ :)**
    - scene이 background로 전환되었을 때 호출
    - 해당 메소드를 사용하여 scene에서 실행 중인 작업 일시 중지 또는 저장 가능

<br><br><br><br>

참고 사이트

---


[https://sueaty.tistory.com/134](https://sueaty.tistory.com/134)

[https://zeddios.tistory.com/811](https://zeddios.tistory.com/811)


<br>

### + Q&A

Q. Scene Delegate를 설명하는 ios 13 사진에서 App Delegate 내부의 Session Lifecycle은 Scene 세션을 관리하는 건가요? 다른 컨셉의 세션일까요

A. Scene 세션이 맞습니다 App Delegate > Session Lifecycle은 Scene Session을 관리하는 것으로 나와있고,두 life cycle 모두 Scene Session을 관리하고 있습니다.

<br>

**추가)**

### **life cycle 별 Scene Session 역할**

App Delegate

- 모든 Scene의 상태를 저장하고 복원

Scene Delegate

- 해당되는 Scene의 상태를 저장하고 복원
