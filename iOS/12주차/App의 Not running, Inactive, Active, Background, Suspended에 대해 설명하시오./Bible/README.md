# App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오.

Not running, Inactive, Active, Background, Suspended는 iOS 앱의 라이프 사이클 상태 구성들입니다.

**1. Not Running**
앱이 실행되지 않은 상태

**2. Inactive**
앱이 실행 중이지만, 이벤트를 받지 않는 상태

**3. Active**
앱이 실행되고 이벤트를 받을 수 있는 상태

**4. Background**
앱이 백그라운드에서 실행되고 있고 일부 코드를 계속 실행할 수 있는 상태

**5. Suspended**
앱이 백그라운드에서 실행되고 있지만 실행 중인 코드가 없는 상태

이러한 앱의 5가지의 상태들이 변화할 때, UIKit은 적절한 위임자 객체의 메소드를 호출하게 됩니다.  
iOS 13부터 SceneDelegate가 추가되었기 때문에 이 기점으로 조금의 차이가 있습니다.

iOS 13 이후의 버전과 Scene을 지원하는 앱은 UISceneDelegate 객체의 메소드를 사용하고,  
iOS 13 이전의 버전과 Scene을 지원하지 않는 앱은 UIApplicationDelegate 객체의 메소드를 사용하게 됩니다.

1. 앱 실행
- iOS 13 이전
앱이 실행되면 UIApplicationDelegate 객체의 application(_:didFinishLaunchingWithOptions:) 메소드를 호출하여 앱을 초기화
- iOS 13 이후
앱이 실행되면 시스템은 UISceneDelegate 객체의 scene(_:willConnectTo:options:) 메소드를 호출하여 초기화

2. 앱 포그라운드 진입
- iOS 13 이전
UIApplicationDelegate 객체의 applicationDidBecomeActive(_:) 메소드 호출
- iOS 13 이후
UISceneDelegate 객체의 sceneDidBecomeActive(_:) 메소드 호출

3. 앱 백그라운드 진입
- iOS 13 이전
UIApplicationDelegate 객체의 applicationWillResignActive(_:) 메소드 호출
- iOS 13 이후
UISceneDelegate 객체의 sceneWillResignActive(_:) 메소드 호출

4. 앱 백그라운드에서 실행
- iOS 13 이전
UIApplicationDelegate 객체의 applicationDidEnterBackground(_:) 메소드 호출
- iOS 13 이후
UISceneDelegate 객체의 sceneDidEnterBackground(_:) 메소드 호출

5. 백그라운드에서 다시 포그라운드 진입
- iOS 13 이전
UIApplicationDelegate 객체의 applicationWillEnterForeground(_:) 메소드 호출
- iOS 13 이후
UISceneDelegate 객체의 sceneWillEnterForeground(_:) 메소드 호출

6. 앱 종료
- iOS 13 이전
UIApplicationDelegate 객체의 applicationWillTerminate(_:) 메소드 호출
- iOS 13 이후
UISceneDelegate 객체의 sceneDidDisconnect(_:) 메소드 호출

## 참고
[Managing your app’s life cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)