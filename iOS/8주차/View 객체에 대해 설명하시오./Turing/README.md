# View 객체에 대해 설명하시오.

1. View란?
    - Basic components of the user interface
    - 뷰는 UIView 클래스 또는 UIView 클래스의 하위클래스(Subclass)의 인스턴스로 윈도우의 한 영역에서 콘텐츠를 보여준다

View 란 사용자 인터페이스를 기본으로 구성하는 요소들을 통칭해서 말하는 거구나

그럼 View를 묶는 건 없을까? 저 위에 윈도우의 한 영역이라고?

1. Window란?
    - Managing the View
    - window는 UIWindow의 instance

Window란 View를 묶고, 이벤트 처리행동을 제공하는 객체구나!

그럼 View는 그림 → Window는 그림을 여러개 담고 배치하는 액자라고 기억하면 편하겠지?

View에대해 더 알아보자!

1. View는 자신의 안에 콘테츠를 보여줌과 동시에 다른 뷰를 위한 컨테이너 역할도 합니다. 이때 뷰과 뷰를 포함한다하여 부모와 자식의 관계가 형성됩니다.
    1. 부모 = 슈퍼뷰(superview)
    2. 자식 = 서브뷰(subview) 

  자식이기는 부모는 없는거 알지? 자식이 부모를.. 이겨버려  하하.. 

<img width="619" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-03-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3 26 12" src="https://user-images.githubusercontent.com/85090866/223940441-c0ad7342-741f-447a-a697-5a3ee9a93a5a.png">

1. View 서브뷰(subView)에 반투명을 주면 부모랑 섞어버려! 나는 서브뷰에 흰색을 주고 슈퍼뷰에 검정을 주었어

<img width="1235" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-03-09_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3 30 51" src="https://user-images.githubusercontent.com/85090866/223940562-68e75b02-9d0e-4503-a553-2dd8657f6b3b.png">


뷰는 많이 사용해 봐서 정의 정도만 알고 있으면 될 것 같아

참고 : [https://developer.apple.com/documentation/uikit/views_and_controls](https://developer.apple.com/documentation/uikit/views_and_controls)



