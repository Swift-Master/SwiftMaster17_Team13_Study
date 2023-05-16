# Bounds 와 Frame 의 차이점을 설명하시오.

### [frame](https://developer.apple.com/documentation/uikit/uiview/1622621-frame) ?

- SuperView의 좌표 시스템 안에서 View의 위치와 크기를 나타낸다.
- UIView의 위치나 크기를 설정할 때 사용된다.
- 스토리보드에서 우측에 있는 X, Y 좌표가 frame의 좌표이다.

⇒ 상위뷰를 기준으로 좌표 위치를 정하는 것

### [Bounds](https://developer.apple.com/documentation/uikit/uiview/1622580-bounds) ?

- View의 위치와 크기를 자신만의 좌표 시스템 안에서 나타낸다.
- View의 크기를 알고 싶거나 View 내부에 그림을 그릴 때 사용된다.

⇒ 자기 자신을 기준으로 좌표 위치를 정하는 것 (상위뷰와는 상관없음)

---

### 사용 시

frame: 부모 뷰의 왼쪽 상단 모서리에서 (20, 20) 위치에 너비 200, 높이 300인 뷰를 표시 할 수 있다.

```swift
view.frame = CGRect(x: 20, y: 20, width: 200, height: 300)
```

bounds: 뷰 자체의 로컬 좌표 시스템에서 위치와 크기를 나타낸다.

```swift
let viewWidth = view.bounds.size.width
let viewHeight = view.bounds.size.height
```

참고 글

---

[https://zeddios.tistory.com/203](https://zeddios.tistory.com/203)
