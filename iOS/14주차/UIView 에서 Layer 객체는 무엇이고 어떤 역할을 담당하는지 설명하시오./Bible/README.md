# UIView 에서 Layer 객체는 무엇이고 어떤 역할을 담당하는지 설명하시오.

UIView의 `Layer` 프로퍼티는 CALayer(Core Animation 프레임워크에서 제공하는 객체) 타입 객체이며 뷰의 그래픽 처리 및 애니메이션 처리를 합니다.

Layer 속성은 항상 CALayer 또는 CALayer 서브클래스 객체를 반환합니다. ⇒ nil이 될 수 없음!  
이 때 객체의 실제 클래스는 UIView의 layerClass 클래스 메서드에서 반환되는 값에 따라 결정이 됩니다.

그리고 UIView가 Layer의 delegate 역할을 합니다.  
UIView는 레이아웃 및 터치 이벤트와 같은 여러 작업들을 수행하지만 직접적으로 화면을 그리거나 애니메이션을 제어하지는 않습니다.

UIView는 CALayer 위에 있는 wrapper라고 보면 됩니다.


![image](https://user-images.githubusercontent.com/59015538/235076872-0239d392-dd31-478e-9bf1-0af0dd0e6cc9.png)


각각의 UIView에는 하나의 루트 CALayer가 있고, 여러 개의 서브레이어를 포함할 수 있습니다.  
이 레이어의 속성으로 View의 그래픽과 애니메이션 처리를 향상시킬 수 있습니다.
```swift
//1
layer.frame = viewForLayer.bounds
layer.contents = UIImage(named: "star")?.cgImage

// 2
layer.contentsGravity = .center
layer.magnificationFilter = .linear

// 3
layer.cornerRadius = 100.0
layer.borderWidth = 12.0
layer.borderColor = UIColor.white.cgColor
layer.backgroundColor = swiftOrangeColor.cgColor

//4
layer.shadowOpacity = 0.75
layer.shadowOffset = CGSize(width: 0, height: 3)
layer.shadowRadius = 3.0
layer.isGeometryFlipped = false
```
layer의 활용 예시 코드입니다.

- 주석 1번
레이어의 프레임을 해당 뷰의 경계에 맞추고 이미지를 배경으로 설정합니다.
- 주석 2번
레이어의 `contentsGravity` 속성에 .center 값을 줘서 내용물이 프레임 가운데에 위치하도록 설정하고,  
`magnificationFilter` 속성은 이미지를 확대하거나 축소할 때 적용되는 필터로 .linear 값을 줘서 선형 보간법으로 확대 혹은 축소합니다.
- 주석 3번
레이어의 외형을 설정하는 코드들인데,  
`cornerRadius`는 레이어의 모서리를 둥글게 만드는 설정으로 100.0 값을 주면 모서리의 반지름이 100 포인트가 됩니다.  
`borderWidth`와 `borderColor`는 레이어의 테두리를 설정합니다. 테두리 두께는 12 포인트, 테두리 색은 흰색으로 설정했습니다.  
`backgroundColor`는 레이어의 배경색을 설정하는 코드입니다.
- 주석 4번
`shadowOpacity`는 레이어의 그림자 투명도를 설정합니다.  
`shadowOffset`는 레이어의 그림자 위치를 설정하는데 CGSize 타입으로 가로와 세로 위치를 지정할 수 있습니다.  
`shadowRadius`는 레이어의 그림자 반경을 설정합니다.  
`isGeometryFlipped`는 레이어의 좌표계를 뒤집을 것인지의 여부를 설정합니다.

이 외에도 다양한 속성들을 활용해서 레이어를 변형시킬 수 있습니다.  
예시 코드는 이런 것들도 있구나~ 가볍게 보시면 될  것 같습니다!

### 출처
- [Layer Apple Documentation](https://developer.apple.com/documentation/uikit/uiview/1622436-layer)
- [CALayer Tutorial for iOS: Getting Started](https://www.kodeco.com/10317653-calayer-tutorial-for-ios-getting-started)
