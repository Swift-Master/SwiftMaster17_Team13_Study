# Delegates와 Notification 방식의 차이점에 대해 설명하시오.

## Delegate 방식이란?
- 두 객체 사이에서 한 객체가 다른 객체가 해야될 동작을 대신하는 것
- [Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.](https://github.com/OpenBible3438/SwiftMaster17_Team13_Study/tree/main/iOS/1%EC%A3%BC%EC%B0%A8/Delegate%20%ED%8C%A8%ED%84%B4%EC%9D%84%20%ED%99%9C%EC%9A%A9%ED%95%98%EB%8A%94%20%EA%B2%BD%EC%9A%B0%EB%A5%BC%20%EC%98%88%EB%A5%BC%20%EB%93%A4%EC%96%B4%20%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4/PAKA) 링크 참조
### 사용자와의 상호작용을 전달할 sender 객체에서는 프로토콜 타입의 대리자 프로퍼티가 존재. 
- sender 내부에는 대리자의 프로토콜 메서드를 실행하는 코드가 존재하여, 대리자 존재시 원하는 동작을 수행.
- 대리자이자 receiver인 객체는 해당 프로토콜을 미리 채택하고, 사용할 프로토콜 메서드를 구현해 놓는다.
### 동작 과정 예시
1. 사용자가 UITextfield에 입력을 시작 
2. 대리자를 호출하는 UITextfield 객체 내부의 메서드가 실행됨 ( 상세구현은 알 수 없다. 기업비밀 🤬)
3. `delegate?.textView(_ shouldChangeCharactersIn...)` 에 의해 대리자가 호출되며 미리 구현해 놓은 동작 수행

## Notification 방식이란?
- 사용자와의 상호작용에 따라 원하는 동작을 다른 객체에서 실행시키는 방식으로, 목적은 delegate와 유사
- 두 객체 사이에 Notification Center라는 경유지 객체가 존재

### 사용법
- User와 상호작용하는 객체 내부에 `NotificationCenter.default.post()`메서드 구현
- 실제 동작을 수행할 객체 내부에 `NotificationCenter.default.addObserver()`메서드 구현 및 옵저버의 타겟 메서드 구현

### 쿠팡 외부 업체 상품 배송을 예시로 이해해보자 (실제 쿠팡 배송과 조금 다를 수 있습니다 🙏🏻)
- User와 객체 1 상호작용(화면터치, 드래그 등..) : 고객이 쿠팡에서 외부 업체 상품 구매
- 객체 1에서 `NotificationCenter로 특정 Notification 전송 : 외부 업체에서 쿠팡물류센터로 상품 인계
```
NotificationCenter.default.post(name : Notification.Name("쿠팡x"), object : 상품, userInfo : [고객정보 : 호갱])
```
- NotificationCenter에서 해당 Notification의 Observer를 가진 객체에게 Notification 전달 : 물류센터에서 상품 분류 후, 고객에게 전달
- `NotificationCenter.default.addObserver()` 내부의 타겟 메서드가 호출되고 원하는 동작 수행 : 택배 수령후 물건 언박싱📦

## 주로 쓰이는 상황
- Delegate 패턴 : 두 객체가 1:1로 연결되는 형태입니다. 프로토콜을 통해 다양한 구현 메서드로 세분화된 로직을 지정 가능!
- Notification 패턴 : 한 객체가 다른 여러 객체와 통신할 떄 사용합니다. Notification 하나당 여러 개의 옵저버를 통해 1:N 통신을 할 수 있다. 그러나, 옵저버와 로직을 구현할 타겟 메서드는 1:1이기 때문에 세밀한 로직은 설정불가
## 장단점(단점은 `블럭안에` 표시)
### Delegate 패턴
- 서로 강하게 연결되어 있고 전달자 객체 내부에 대리자 참조 속성이 있기 떄문에, 데이터 전달이 용이하다.
- `대리자 객체가 많은 세부 기능을 가지기 때문에 결합도가 올라간다.`
### Notification 패턴
- 서로 느슨하게 연결(MVC패턴의 뷰와 모델의 관계처럼, Notification 발송 객체와 Notification 수신 객체는 서로 모릅니다.)
- 하나의 Notification으로 1:N 객체 통신이 가능하고 Notification 전송, 수신 구현이 Delegate에 비해 간단
- `직접 연결되어 있지않아, Notification을 통해 수신한 데이터 외에 수신 객체에서 발신 객체의 데이터를 얻어오기 불가능`
- `Notification당 연결된 옵저버가 많을 수록 관리하기 힘들 수 있고, 수신한 Notification 하나당 메서드 1개만 구현 가능` 
- `해당 객체가 소멸하기 전까지 Notification이 못 오게 막을 수 없습니다. 소멸자나 removeObserver() 등을 통해 제거!`

## 참고 링크
### StackOverFlow
 - [스택오버플로우](https://stackoverflow.com/questions/5325226/what-is-the-difference-between-delegate-and-notification)
### 블로그
- [벨로그](https://velog.io/@tnddls2ek/Swift-Delegate-Notification-%EB%B0%A9%EC%8B%9D%EC%9D%98-%EC%B0%A8%EC%9D%B4)
- [티스토리](https://nsios.tistory.com/33)