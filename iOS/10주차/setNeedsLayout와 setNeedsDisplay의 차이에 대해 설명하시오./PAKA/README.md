# setNeedsLayout와 setNeedsDisplay의 차이에 대해 설명하시오.

## main run loop
- 유저의 input 이벤트를 이벤트 대기열에 추가
- 애플리케이션 객체가 대기열에서 이벤트를 하나씩 꺼내 해석하고 Core object 내부 핸들러를 호출하여 다른 UI객체로 전달, 개발자의 코드를 실행
- 다시 run loop로 복귀해 update 사이클(view 재배치 및 새로 그리기) 시작

<img width="694" alt="스크린샷 2023-03-27 오후 10 33 27" src="https://user-images.githubusercontent.com/116094622/227953382-aacef49f-e759-42cd-81cf-be9db9b2a203.png">

- 이 사이클은 1/60초 안에 완료되지만, 기본적으로 main run loop의 작업은 비동기로 수행됩니다. 따라서 핸들러를 통해 호출된 코드가 view의 수정을 요청하더라도, 이미 update 사이클이 먼저 수행되어 그 다음 update 사이클이 되야 view가 수정될 것입니다.
- 이런 시점 차이로 인해, view의 layout, content를 이용한 조작이나 연산이 필요한 경우 과거 값을 참조하게 될 수도 있습니다!
## 레이아웃
- UIView의 위치와 크기를 의미합니다.
- 레이아웃이 변경되었음을 알려주는 메서드(`viewDIdLayoutSubviews`)와 계산되는 시점(`layoutSubviews()`)에 콜백으로 호출되는 메서드가 존재합니다.
- 정확한 레이아웃이나 위치값을 얻기 위해서는 `viewDIdLayoutSubviews` 에서 작업을 수행해야 합니다.
### layoutSubviews()
- 레이아웃이 계산되는 시점에 호출되는 메서드로, 현재 뷰와 자식 뷰의 layoutSubviews() 메서드를 재귀적으로 모두 호출시킵니다.
- 이 메서드 재정의를 통해 정확한 frame 값을 얻을 수 있으나, 직접 호출은 금지되어 있습니다.
- 간접적으로 layoutSubviews()를 호출하도록 해주는 다양한 메서드를 대신 사용합니다.
	#### Automatic refresh triggers(자동으로 다음 사이클에 호출 보장)
	- 자동으로 view가 다음 사이클에 layoutSubviews()를 호출하도록 만들어주는 이벤트들입니다.
	- 종류
		- view resizing
		- subView 추가
		- UIScrollView 스크롤시 UIScrollView와 슈퍼뷰의 layoutSubviews() 호출
		- 기기 회전
		- view의 Constraint 변경
	#### setNeedsLayout(이상적인 방법)
	- 가장 적은 부하로 layoutSubviews()를 호출할 수 있는 메서드입니다.
	- 이 또한 즉시 layoutSubviews()가 호출되지는 않으며, 다음 update 사이클에서 `setNeedsLayout`이 호출된 뷰와 그 자식 뷰에서만 layoutSubviews()가 호출됩니다.
	#### layoutIfNeeded(즉시 반영되야 할 때)
	- 즉시 layoutSubViews()를 호출할 수 있는 메서드입니다.
	- 메서드가 반환되기 전 레이아웃 대상 뷰와 그 자식 뷰들을 다시 그립니다.(애니메이션 제외)
	- 단, 전제 조건으로 view의 layout이 재조정이 필요해야 합니다.
		- setNeedsLayout이나 Automatic refresh triggers를 통해 레이아웃의 업데이트를 필요로 하는 작업  이후에 호출함으로써 즉시 레이아웃을 변경할 수 있습니다!
		- 응용으로, 레이아웃 업데이트가 필요한 상황에서 layoutIfNeeded를 두번 호출시, 마지막 호출은 아무 동작도 하지 않습니다😄