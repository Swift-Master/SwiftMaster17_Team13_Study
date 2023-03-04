# TableView와 CollectionView의 차이점을 설명하시오

## UITableView에 대하여..
- cell이라는 하나의 단위로 정보를 전달하는 뷰로, Row와 Section으로 구역을 나눈다.
- `UIScrollView`의 자손 클래스로 상하로 스크롤이 가능하다.
- 사용하기 위해서는 `UITableViewDataSource` 프로토콜을 필수적으로 채택하여 구현해야 한다.
- 기본적으로 제공되는 cell을 사용할 수도 있고 custom하여 사용할 수도 있다. 
### 주요 프로퍼티, 메서드
- `IndexPath` : 테이블뷰의 Row와 Section 정보가 담긴 구조체이다.
- `dataSource, delegate` : 델리게이트 객체. 데이터소스의 델리게이트는 구별을 위해 이름이 다름.
- `rowHeight` : cell의 높이를 설정할 수 있다.
- `reloadData` : 테이블뷰에 들어가는 데이터가 담긴 배열에 변동(삭제, 추가, 수정)이 있을 때 사용.
- `.dequeueReusableCell` : 메모리 절약을 위해 셀을 재사용할 수 있는 메서드
    - 셀 재활용 → cell은 생성할 떄마다 힙에 올라감(클래스), 이로 인해 테이블을 스크롤할때마다 새 cell을 만드는건 메모리낭비
    - 리턴타입은 UITableViewCell이라는 기본 프로토타입이기 때문에 커스텀셀 사용시 커스텀 셀 클래스 이름으로 다운캐스팅해준다.(as!)
- `cell.selectionStyle` : 셀이 선택되는 색을 정할 수 있다.## UITableView에 대하여..
- cell이라는 하나의 단위로 정보를 전달하는 뷰로, Row와 Section으로 구역을 나눈다.
- `UIScrollView`의 자손 클래스로 상하로 스크롤이 가능하다.
- 사용하기 위해서는 `UITableViewDataSource` 프로토콜을 필수적으로 채택하여 구현해야 한다.
- 기본적으로 제공되는 cell을 사용할 수도 있고 custom하여 사용할 수도 있다. 
### 주요 프로퍼티, 메서드
- `IndexPath` : 테이블뷰의 Row와 Section 정보가 담긴 구조체이다.
- `dataSource, delegate` : 델리게이트 객체. 데이터소스의 델리게이트는 구별을 위해 이름이 다름.
- `rowHeight` : cell의 높이를 설정할 수 있다.
- `reloadData` : 테이블뷰에 들어가는 데이터가 담긴 배열에 변동(삭제, 추가, 수정)이 있을 때 사용.
- `.dequeueReusableCell` : 메모리 절약을 위해 셀을 재사용할 수 있는 메서드
    - 셀 재활용 → cell은 생성할 떄마다 힙에 올라감(클래스), 이로 인해 테이블을 스크롤할때마다 새 cell을 만드는건 메모리낭비
    - 리턴타입은 UITableViewCell이라는 기본 프로토타입이기 때문에 커스텀셀 사용시 커스텀 셀 클래스 이름으로 다운캐스팅해준다.(as!)
- `cell.selectionStyle` : 셀이 선택되는 색을 정할 수 있다.

## UICollectionView에 대하여..
- ios 6 이후 등장, 기존 테이블 뷰의 거의 대부분의 동작 + 부가적인 많은 기능
- 기존의 cell에 부가적으로 html 시멘틱 문법처럼 header와 footer 추가되어 커스텀이 더 쉬워짐
- 마찬가지로 ScrollView 상속, 가로/세로 스크롤 모두 가능
- `UICollectionViewFlowLayout`을 통해 다양한 레이아웃 지정이 가능합니다.
- 셀 내부에 뷰를 넣지않고도 개별 뷰를 셀처럼 사용할 수 있습니다!
- 기본적으로 제공되는 cell이 없다
### 주요 프로퍼티, 메서드
- `supplementaryViewProvider` : header, footer와 같은 보조 뷰 구현시 사용하는 프로퍼티
- `UICollectionViewDiffableDataSource` : 기존의 dataSource보다 업그레이드 된 데이터 관리 방식 
- `reorderingHandlers`: 셀을 재정렬할 때 사용하는 프로퍼티

## 주요 차이점 정리

|특징  | UITableView | UICollectionView  |
|--|--|--|
| 기본 셀 유무 |  O| x |
| 스크롤 가능 방향 |  수직| 수직, 수평 |
| 열, 행 |  1개의 열, 여러개의 행| 여러 개의 열, 여러 개의 행 |
| 레이아웃 변경 |   | UICollectionViewDelegateFlowLayout |

## 관련 링크
- [CollectionView와 TableView 언제 사용해야 되나](https://k-d-a.medium.com/when-to-use-a-collectionview-and-tableview-246d8270698d)
- [UICollectionView를 이용한 LINE iOS 대화방 리팩토링 - 1](https://engineering.linecorp.com/ko/blog/ios-refactoring-uicollectionview-1)
- [애플 공식 문서 - 컬렉션 뷰](https://developer.apple.com/documentation/uikit/uicollectionview)
- [애플 공식 문서 - 테이블 뷰](https://developer.apple.com/documentation/uikit/views_and_controls/table_views) 