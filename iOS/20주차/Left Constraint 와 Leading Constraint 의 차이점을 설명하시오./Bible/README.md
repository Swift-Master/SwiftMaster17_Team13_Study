# Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.

Left Constraint는 왼쪽을 기준으로 제약 조건을 나타냅니다.

한글을 포함한 대부분의 언어들은 왼쪽에서 오른쪽으로 읽기 때문에 Left Constraint를 사용해도 문제가 되지 않습니다.

하지만 오른쪽에서 왼쪽으로 읽는 언어(아랍어, 히브리어 등등)들이 존재하기 때문에 애플에서는 Left Constraint 사용보다는 Leading Constraint 사용을 권장하고 있습니다.

Leading Constraint는 사용자가 설정한 현재 언어에 따라 읽기 방향을 결정해서 뷰를 배치하게 합니다.

<img width="372" alt="사진1" src="https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/59015538/e23d56d0-256b-48ab-bca1-a56ad9474d19">
<img width="369" alt="사진2" src="https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/59015538/4e612155-65f3-44d8-9ba1-3662018e4e6b">

아랍어로 설정하게 되면 버튼, 글자 등등 뷰들의 위치가 오른쪽을 기준으로 정렬되는 것을 볼 수 있습니다.

다국어를 지원하는 앱이라면 이 부분에 특히 유의해야될 것 같습니다 :)

[Sueaty - Left Constraint와 Leading Constraint의 차이](https://sueaty.tistory.com/175) 이 링크에서 사용법까지 훨씬 친절하게 설명해주셨으니 참고하시면 될 것 같습니다!