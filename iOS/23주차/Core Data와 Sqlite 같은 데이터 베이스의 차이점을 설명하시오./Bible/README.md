# Core Data와 Sqlite 같은 데이터 베이스의 차이점을 설명하시오.

## Core Data
- 단일 장치에서 데이터를 저장, 캐시해서 오프라인에서도 앱의 데이터를 사용할 수 있고, CloudKit(iCloud)으로 데이터를 저장해서 여러 장치에서 데이터를 동기화, 미러링 할 수 있도록하는 애플의 프레임워크입니다.
- Core Data의 저장소에 접근하는 방식은 모두 추상화 되어 있기 때문에 복잡한 SQL 쿼리문을 작성 할 필요없이 제공되는 인터페이스를 사용하여 Swift나 Objective-C로 데이터를 관리할 수 있습니다.
    - Object-Oriented 방식으로 데이터를 다룹니다. Object Graphs라고 표현하는데, 파일 형태로 저장되어 있는 데이터를 지정된 객체와 연결하는 것을 의미합니다. Core Data를 사용하여 데이터를 불러오면 객체 형태로 데이터를 받고, 이 객체의 속성을 수정해서 다시 데이터를 저장할 수 있습니다.
- Core Data의 Undo Manager를 사용하여 사용자가 일련의 작업의 실행을 취소하거나 다시 실행하는 기능을 제공합니다.
    - 핸드폰을 흔들거나 Undo 기능을 사용하도록 해서 작업 이전의 상태로 실행 취소(롤백) 시키는 것이 가능하고, Redo 기능으로 실행 취소한 작업을 재실행시킬 수 있습니다.
- 백그라운드에서 데이터 처리 작업이 가능합니다.
- 서버로부터 받은 데이터를 Core Data에 캐시해서 동일한 데이터를 요청할 때 서버없이 로컬에서 바로 사용할 수 있도록 합니다.
- TableView, CollectionView에 대한 데이터 소스를 제공해서 View와 데이터 동기화하는데에 도움을 줍니다.
- 앱의 버전이 변화하고 발전함에 따라 데이터 모델도 마이그레이션 할 수 있도록 매커니즘이 포함되어 있습니다.

## SQLite
- 서버가 아닌 응용 프로그램에 넣어 사용하는 비교적 가벼운 관계형 데이터베이스로, 애플에서 제공하는 프레임워크가 아닌 C언어로 만들어진 라이브러리입니다.
- iOS와 함께 제공되므로 앱 번들에 부담을 주지 않습니다.
    - 별도의 설치나 설정없이 즉시 데이터베이스를 사용할 수 있습니다.
- 2000년 8월에 출시되어 지속적으로 업데이트되고 있으며 안정성을 인정받은 데이터베이스 엔진입니다.
- 오픈 소스이기 때문에 누구나 자유롭게 사용하고 수정할 수 있습니다. 개발자들에게 커스터마이징 가능성을 제공합니다.
- SQL를 사용하여 데이터베이스를 쿼리합니다.
- 크로스 플랫폼으로 다른 플랫폼 간에 데이터를 이동하고 공유할 수 있습니다.

## Core Data와 SQLite 차이점
SQLite는 `Lightweight Solution`이 필요할 때, Core Data는 `Complex Object Graph`가 필요할 때 사용하는 것이 적절합니다.

Core Data는 Object Graph를 메모리에 올려놓고 사용하게 됩니다. 따라서 Object를 다루는 작업을 한 번에 많이 한다던가, 빈번하게 작업한다면 메모리 사용으로 인한 퍼포먼스 이슈가 생길 수 있습니다.

속도면에서는 코어 데이터가 더 빠르게 데이터를 가져올 수 있지만 메모리 및 저장공간 또한 더 많이 사용합니다.

또한 Core Data는 데이터베이스가 아니기 때문에 Core Data가 내부적으로 SQLite를 사용하기도 합니다.

### 출처
- [Core Data - Apple Developer Documentation](https://developer.apple.com/documentation/coredata#3073805)
- [Core Data - 다른 저장소와의 비교](https://yeonduing.tistory.com/54)
- [SQLite With Swift Tutorial: Getting Started](https://www.kodeco.com/6620276-sqlite-with-swift-tutorial-getting-started)
