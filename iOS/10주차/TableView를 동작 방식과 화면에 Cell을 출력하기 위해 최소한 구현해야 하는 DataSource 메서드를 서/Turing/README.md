# TableView를 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오.

마지막.. 글을 남겨보네요.

자 TableViews는 다들 사용해봤으니 알테구 그럼 동작방식과 최소 구동조건을 바로 알아보자!

1. TableView는 DataSource 객체로부터 필요한 셀 정보를 요청!
2. DataSource 객체는 정보의 요청이 들어오면 → 셀 정보를 제공
3. TableView는 DataSource 객체로부터 제공된 정보를 바탕으로 셀을 만들고 표시!

그러면 최소 동작조건을 만족하는 코드로 바로 봐보자

```swift
class ViewController: UIViewController, UITableViewDataSource {

    // TableView의 데이터 소스로 사용될 배열
    let items = ["Apple", "Banana", "Cherry", "Durian"]

    // TableView에서 표시될 셀의 개수를 반환하는 메서드
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return items.count
    }

    // TableView에 표시될 각 셀을 구성하고 반환하는 메서드
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
        cell.textLabel?.text = items[indexPath.row]
        return cell
    }
}
```

최소 구현 메서드가 몇개냐면 2개야!

1. numberOfRowsInSection: 이 메서드는 TableView에 표시될 셀의 개수를 반환
2. cellForRowAt: 이 메서드는 TableView에 표시될 각 셀을 구성하고 반환

끝 아주 간단하지 ?

근데.. 좀 더 자세하게 알고 싶단말이지! (사실 위에가 끝인데 좀 더 자세히 설명해볼게)

TableView는 IOS앱에서 사용되는 UI 컴포넌트이고, 리스트 형식으로 데이터를 보여주는 역할을 해 

구동방식을 세분화 하면

1. 데이터의 요청 

- TableView는 DataSource 객체에게 필요한 데이터를 요청
- 데이터 요청은 TableView에서 몇 개의 셀이 필요한지 먼저 물어보자!
- numberOfRowsInSection 메서드를 통해 필요한 셀의 개수를 반환
- 그 다음, cellForRowAt 메서드를 호출하여 각 셀의 데이터를 요청
- cellForRowAt 메서드는 IndexPath 객체를 인자로 받아 해당 위치에 있는 데이터를 반환
1. 데이터 표시
    - TableView는 DataSource로부터 요청한 데이터를 이용해 셀을 구성합니다.
    - 구성된 셀은 화면에 표시됩니다.

최소 구현을 세분화 하면

1. numberOfRowsInSection 메서드
    - 이 메서드는 TableView에 표시될 셀의 개수를 반환합니다.
    - 예를 들어, 데이터 배열의 count를 반환하면 됩니다.
2. cellForRowAt 메서드
    - 이 메서드는 TableView에 표시될 각 셀을 구성하고 반환합니다.
    - dequeueReusableCell(withIdentifier:for:) 메서드를 이용하여 재사용 가능한 셀을 가져오고, 새로운 셀을 생성할 경우에는 셀을 생성합니다.
    - 각 셀은 textLabel이나 imageView 등의 속성을 이용하여 구성할 수 있습니다.
    - 마지막으로 구성된 셀을 반환합니다.
    

이렇게 된다구 🙂

Bible, Paka, Leeds 모두 정말 부족한 저를 챙겨주고 .. 끝까지 잘 이끌어주셔서 감사했습니다. 저는 이 스터디에서 정말 많은 걸 배우고 갑니다 ! 
다음에는 현직에서 보구.. 꼭 만나실때 저 불러주시면 바로 달려나갈게요 ㅠㅠ.. 정말루 서울도 갈 수 있습니다.
나중에 꼭 봐요
