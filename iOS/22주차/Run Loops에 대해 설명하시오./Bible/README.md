# Run Loops에 대해 설명하시오.

Run Loop는 스레드와 관련된 기본 인프라입니다.

작업이 있다면 스레드를 사용해서 작업을 처리하고, 작업이 없다면 스레드를 대기상태로 만들어줍니다.

자동적으로 이루어지진 않기 때문에 Cocoa, Core Foundation에서는 런 루프 객체를 제공하여 개발자가 직접 코드를 작성해야 합니다.

런 루프는 두 가지 유형의 소스를 받게 됩니다.

- Input Source : 다른 스레드나 다른 응용 프로그램의 비동기 이벤트가 수신된 것을 확인하고 이벤트 핸들러 수행  
(마우스 및 키보드 이벤트와 같은 사용자 입력, 네트워크 연결, 프로세스 간의 통신을 위한 포트 이벤트 등등의 유형이 있음)
- Timer Source : 예정된 시간이나 반복되는 간격으로 발생하는 동기 이벤트를 수신된 것을 확인하고 이벤트 핸들러 수행

이런 상황에서 `current` 메서드를 사용해서 런 루프를 생성하고 이벤트를 처리하면 됩니다.

```swift
let runLoop = RunLoop.current
```

그리고 4가지의 메서드를 사용해서 런 루프를 실행시켜줘야 합니다.

<img width="851" alt="사진1" src="https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/59015538/a59e314f-799f-4077-9118-bee2c512a3c8">

이 메서드들을 한 번 실행했다고 해서 런 루프가 계속 돌아가진 않습니다.  
while, for와 같은 루프 안에서 반복적으로 실행하면서 이벤트들을 받고 핸들러를 실행할 수 있도록 해주어야 합니다.

### 출처
- [Run Loop - Apple Developer Documentation](https://developer.apple.com/documentation/foundation/runloop)
- [Run Loops - Apple Documentation Archive](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html)