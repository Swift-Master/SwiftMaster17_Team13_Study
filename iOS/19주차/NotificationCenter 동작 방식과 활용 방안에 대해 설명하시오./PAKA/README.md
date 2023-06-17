# NotificationCenter 동작 방식과 활용 방안에 대해 설명하시오.

## Notification의 특징
- 이벤트(ex : 화면이 포커스, 네트워크 연결 종료..)에 대한 정보를 캡슐화한 객체
- 객체 간 정보 전달 시 메서드 호출 방식은 강한 결합을 유발하기 때문에 중간 객체(NotificationCenter)를 거치는 방식을 사용합니다.
- 알림 게시자, 관찰자 객체에는 제한이 없으며 서로 같게 만들 수도 있습니다.
- 알림 게시자는 관찰자의 정보를 알 필요가 없고, 관찰자 또한 게시자를 모르며 알림 센터를 통해 알림 이름과 딕셔너리 키값만 전달받습니다.

### Notification의 구성요소
- name : Notification 식별 태그
- object : Notification 게시자가 그 Notification의 Observer에게 보내는 객체로, 알림 자체를 의미
- Optional 타입 딕셔너리 : Notification을 트리거하는 이벤트에 대한 추가정보가 포함된 컬렉션 타입 객체

#### Notification 전달 과정 흐름도

<p align = middle><image src = "https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/116094622/6544ad5d-cd91-426a-b865-accae625587a" width = 70%></image></p>

- 이벤트 발생을 알고 싶은 관찰자 객체(Observer)는 해당 이벤트가 발생시 알림을 받기 위해 알림 센터에 등록
- 이벤트가 발생하면 알림 센터에 알림이 게시(Post)되고 등록된 모든 개체에 알림이 즉시 Broadcast됩니다. 
- 특별한 목적(비동기 알림, 알림 통합)을 위해 알림은 알림 대기열(Notification Queue)로 Post되기도 합니다.

### Notification vs Delegate


|특징|Notification|Delegate|
|----|----|-----|
|데이터수신형태|단방향(게시자->관찰자)|쌍방향|
|객체 개수|1(알림) : N(관찰자 객체)|1:1|
|사전 정의 메서드|필요 없음| 프로토콜 메서드 구현 필요|
|결합도|상대적으로 낮음(게시자와 관찰자는 서로 모름)|상대적으로 높음

## Notification Center

### 특징
- 알림 전송 및 수신을 관리하며, 환경에 따라 두 종류가 있습니다. 
	- `NotificationCenter`클래스 : 단일 프로세스 내에서 알림을 관리
	- `DistributedNotificationCenter`클래스 : 단일 컴퓨터의 여러 프로세스에서 알림을 관리
- 프로세스에서 NotificationCenter.default라는 연산 프로퍼티를 통해 기본 제공 NotificationCenter 객체에 액세스할 수 있습니다.
- 알리 전달 및 수신의 기본 동작은 동기적으로 일어납니다. 만약 비동기적으로(알림을 보낸 이후 즉시 제어권 반환) 알림을 보내려면 Notification Queue를 사용해야 합니다.
- 알림은 항상 게시된 쓰레드에서 전달되고 알림 관찰 객체가 등록된 쓰레드와 다를 수 있습니다.

### NotificationCenter.default
- 앱의 모든 시스템 알림은 기본 제공 알림센터에 게시됩니다.(커스텀 알림 포함)
-  알림 센터는 등록된 관찰자 목록을 모두 스캔하므로 앱 속도에 영향을 미칩니다. 따라서 알림과 관찰자의 수가 많다면 추가로 알림 센터 객체를 생성하여 성능 영향을 최소화시킬 수 있습니다.

### DistributionNotificationCenter(분산 알림센터)
- 일반 알림센터와 동작은 비슷하나 시스템 전체 서버를 통해 모든 프로세스의 알림을 배포하고 그 알림이 다른 프로세스에 도착 시간이 무제한이기 때문에 비용이 많이 들고 너무 많은 알림으로 대기열이 과부하되면 알림이 삭제되기도 합니다.
- 프로세스의 런루프를 통해 알림을 전달받게 됩니다. 다중 쓰레드 환경의 프로세스는 메인 런루프가 아닐 수 있습니다.
- 일반 알림센터와 달리 문자열 객체만 관찰 가능합니다(프로세스가 달라 객체 포인터가 포함될 수 없음).

## NotificationCenter의 활용방안

- 하나의 이벤트에 대해 변경이 필요한 객체들이 많고 쌍방향 통신이 필요없다면 Notification Center를 활용한 정보전달 방식이 유용합니다.
- 새로운 알림을 생성하여 post하는 것도 가능하지만, 주로 UIWindow, UIApplication 등의 시스템 객체들이 사용하는 알림의 observer를 등록하여 원하는 시점에 정보를 얻거나 변경할 수 있습니다.

### [UIWindow의 Notification 활용](https://github.com/Swift-Master/SwiftMaster17_Team13_Study/blob/main/iOS/14%EC%A3%BC%EC%B0%A8/UIWindow%20%EA%B0%9D%EC%B2%B4%EC%9D%98%20%EC%97%AD%ED%95%A0%EC%9D%80%20%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%3F/PAKA/README.md)
- 앱의 상태(suspended, active, inactive, background,not running)에 따라 notification을 post하고 자기 자신의 속성을 동적으로 변화시킵니다. -> 상태가 변하는 시점에 원하는 정보를 얻거나 화면 변경하기 가능
- 키보드 관련 이벤트 발생 시 알림 센터로 알림을 post하여 관련 UI객체(UIScreen, UIResponder..)의 속성들을 동적으로 변화시킵니다. -> 키보드가 내려갈 때 form 유효성 검사 등의 로직 설정 가능

## 참고문헌

- [애플 문서 아카이브 - 알림 프로그래밍 가이드](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Notifications/Introduction/introNotifications.html#//apple_ref/doc/uid/10000043-SW1)
- [애플 공식문서 - Notification Center](https://developer.apple.com/documentation/foundation/notificationcenter)

