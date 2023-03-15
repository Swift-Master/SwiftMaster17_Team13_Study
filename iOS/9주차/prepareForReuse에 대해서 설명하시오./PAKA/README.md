# PrepareForReuse()에 대해서 설명하시오.

## prepareForReuse()란?
- UITableView 또는 UICollectionView 사용 시 메모리 절약을 위해 `dequeueReusableCell` 메서드를 사용하여 cell을 재사용하고 있습니다.
- 테이블뷰의 셀 재사용 과정은 다음과 같습니다.
  -  UITableView의 register() 메서드를 통해 재사용할 테이블셀을 미리 지정합니다.
  `tableView.register(재사용 셀 클래스의 메타타입, forCellReuseIdentifier: "재사용셀이름")`
  - 화면에서 보여지는 셀의 개수는 셀의 높이(`rowHeight`)에 따라 달라지고, 스크롤 시 새 셀이 보여져야 할 때 현재 화면 가장 상단에 존재하는 셀이 대기열로 들어가게 됩니다.
  - 이후 cell의 property 값을 설정해주는 `cellForRowAt` 메서드에서 `dequeueReusableCell`을 호출해서 대기열의 셀을 가져와서 재사용합니다.
  - `dequeueReusableCell`을 통해 셀을 가져오기 전 `prepareForReuse` 메서드가 먼저 호출되고, 이름 그대로 미리 셀에 대한 설정을 해줄 수 있습니다.

## 그래서 언제 사용할까?
- 테이블뷰나 컬렉션 뷰에서 이미지를 네트워킹을 통해 받아오는 경우, 네트워크 환경이나 동시성 오류 등으로 이미지 로드가 실패하기도 합니다. 그런 경우 `cellForRowAt` 에 바꿔줄 이미지가 존재하지 않기 때문에 기존 이미지가 그대로 사용될 수 있어 `prepareForReuse` 에서 로드되지 않았을 때 사용할 기본 이미지를 설정해 줄 수도 있습니다.
- 유저와의 상호작용으로 나중에 변화가 생기지만  `cellForRowAt` 에서 설정하지 않는 속성들도 있습니다.  이러한 부분들에 대해 따로 설정하지 않으면 재사용되는 모든 셀에서 해당 속성이 남아있을 수 있습니다. `prepareForReuse` 를 통해 관련 속성들을 초기화시키는 방법을 사용할 수 있습니다.

## 참고링크
- [dequeueReusableCellWithIdentifier 과정과 사용이유](https://sihyungyou.github.io/iOS-dequeueReusableCell/)
- [prepareForReuse란?](https://sueaty.tistory.com/180)