# 오토레이아웃을 코드로 작성하는 방법은 무엇인가? (3가지)

### **Auto Layout ?**

- 제약조건(Constraints)에 따라 view의 크기와 위치를 동적으로 지정하는 것

---

<br>

Auto Layout을 코드로 작성하는 방법에는 3가지 방법이 있습니다.

1. Layout Anchor
2. NSLayoutConstraint class
3. Visual Format Language

<br>

****Layout Anchor****

- 제약을 주고 싶은 item의 anchor property에 접근해서 제약을 정의하는 방식

<br>

```swift
// "view1"의 가장자리를 슈퍼뷰와 맞추는 코드 예시

let view1 = UIView()
view1.translatesAutoresizingMaskIntoConstraints = false  // 아래 설명
view.addSubview(view1)

view1.leadingAnchor.constraint(equalTo: view.leadingAnchor).isActive = true
view1.trailingAnchor.constraint(equalTo: view.trailingAnchor).isActive = true
view1.topAnchor.constraint(equalTo: view.topAnchor).isActive = true
view1.bottomAnchor.constraint(equalTo: view.bottomAnchor).isActive = true
```

`translatesAutoresizingMaskIntoConstraints = false`  자동 크기 및 위치 조정 사용 X

`translatesAutoresizingMaskIntoConstraints = true` 는 크기 및 위치 자동 조정 O

우리는 코드로 제약을 정의할 것이기 때문에 false로..

<br><br>

**NSLayoutConstraint**

- NSLatoutConstraint class를 사용하여 뷰의 제약 조건 작성
- Layout에 관여하지 않는 파라미터도 모두 값을 지정해주어야 함

<br>

```swift
// "view1"의 가장자리를 슈퍼뷰와 맞추는 코드 예시

let view1 = UIView()
view1.translatesAutoresizingMaskIntoConstraints = false
view.addSubview(view1)

let leadingConstraint = NSLayoutConstraint(item: view1, attribute: .leading, relatedBy: .equal, toItem: view, attribute: .leading, multiplier: 1, constant: 0)
let trailingConstraint = NSLayoutConstraint(item: view1, attribute: .trailing, relatedBy: .equal, toItem: view, attribute: .trailing, multiplier: 1, constant: 0)
let topConstraint = NSLayoutConstraint(item: view1, attribute: .top, relatedBy: .equal, toItem: view, attribute: .top, multiplier: 1, constant: 0)
let bottomConstraint = NSLayoutConstraint(item: view1, attribute: .bottom, relatedBy: .equal, toItem: view, attribute: .bottom, multiplier: 1, constant: 0)

NSLayoutConstraint.activate([leadingConstraint, trailingConstraint, topConstraint, bottomConstraint])
```

<br><br>

**Visual Format Language**

- 문자열 형식으로 제약 조건 작성

```swift
// "view1"의 가장자리를 슈퍼뷰와 맞추는 코드 예시

let views = ["view1": view1]
let constraints = NSLayoutConstraint.constraints(withVisualFormat: "H:|-[view1]-|", options: [], metrics: nil, views: views)
NSLayoutConstraint.activate(constraints)

let constraints2 = NSLayoutConstraint.constraints(withVisualFormat: "V:|-[view1]-|", options: [], metrics: nil, views: views)
NSLayoutConstraint.activate(constraints2)
```

`H:` ⇒ 가로 방향의 제약조건

`V:` ⇒ 세로 방향의 제약 조건

`|`   ⇒ 슈퍼뷰의 가장자리를

`-`   ⇒ 간격

<br>

`H:|-[view1]-|` 을 풀어서 정리하자면 **가로방향 슈퍼뷰의 가장자리의 양쪽 간격** 이다.

<br>

간격 조절은 아래 코드처럼 작성 할 수 있다!

```swift
let views = ["view1": view1]
let metrics = ["gap": 20]
let constraints = NSLayoutConstraint.constraints(withVisualFormat: "H:|-(gap)-[view1]-(gap)-|", options: [], metrics: metrics, views: views)
NSLayoutConstraint.activate(constraints)

let constraints2 = NSLayoutConstraint.constraints(withVisualFormat: "V:|-(gap)-[view1]-(gap)-|", options: [], metrics: metrics, views: views)
NSLayoutConstraint.activate(constraints2)
```

<br><br>

최근에는 [Layout Anchor를 사용하는 것을 권장](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/WorkingwithConstraintsinInterfaceBuidler.html#//apple_ref/doc/uid/TP40010853-CH10-SW1)한다고 합니다!

1. 가독성: 간단하고 직관적인 API를 제공하여 코드를 작성하는 것을 쉽게 만듬
2. 유지보수: 컴파일러가 타입 검사를 수행하기 때문에 일부 오류 미리 감지 가능
3. 성능: NSLayoutConstraint보다 조금 더 빠름

<br><br><br><br>

참고

---

[https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)

[https://sueaty.tistory.com/174](https://sueaty.tistory.com/174)
