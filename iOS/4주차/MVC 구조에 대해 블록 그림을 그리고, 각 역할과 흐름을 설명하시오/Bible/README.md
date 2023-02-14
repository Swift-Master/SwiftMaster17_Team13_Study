# MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.

MVC 구조는 MVC 디자인 패턴이라고도 하는데요!
MVC, `Model-View-Controller`로 성격에 따라 로직(코드)을 분리해서 틀을 잡은 소프트웨어 설계 방식입니다. 이말은 곧 아키텍쳐 방식이기도 합니다.

이런 설계 방식을 사용하는 이유는 비즈니스 로직과 인터페이스 요소들을 분리해서 서로 영향이 가지 않게 개발하다 위입함니닏. 어 느한한파에일이 것이것 온갖 코드들이 다 들어가있으면 가독성도 떨어지고 나 말고 다른 개발자가 와서 코드를 볼 때 읽기도 어렵겠죠. 즉 유지보수!

iOS 앱을 만들 때는 MVC 패턴 방식 외에도 MVVM이나 MVP 등등의 디자인 패턴을 적용하는데, MVC 패턴은 iOS 같은 모바일 앱을 만드는데에도 자주 쓰이고 웹 개발에서도 아주 아주 자주 쓰이는 패턴 중 하나입니다.

## MVC 구조

![Untitled](https://user-images.githubusercontent.com/59015538/216318728-840cb3a9-da32-4852-9317-1ce484eeec84.png)

사진 출처 - [https://www.geeksforgeeks.org/benefit-of-using-mvc/](https://www.geeksforgeeks.org/benefit-of-using-mvc/)

개인적으로 MVC 패턴을 간결하게 잘 나타낸 사진인 것 같아서 가져왔습니다.

먼저 User는 Controller에 데이터를 달라고 요청(Request)을 합니다. 이 요청은 쇼핑몰 앱에서 카테고리 중 ‘상의’를 눌렀다면 상의 데이터들만 달라고 요청을 한 것이죠.

그러면 Controller는 Model에게 ‘상의’ 데이터를 달라고 요청을 하고, Model은 Controller가 요청한 데이터를 가져와서 전달합니다. Model은 DB와 밀접하게 있다고 생각하면 됩니다.

Model에게 데이터를 받은 Controller는 View에게 ‘상의’ 데이터를 받았으니 User가 볼 수 있도록 화면에 이 데이터들을 뿌려달라고 합니다. 그럼 View는 화면에 데이터들을 담아서 User에게 최종적으로 보여주게 됩니다.

## 역할

### Model

- 앱의 데이터와 관련된 내용과 데이터 관리 로직을 가짐
- VO, DTO, DAO 등등의 구조체와 데이터를 받기 위한 네트워크 접근 로직들이 이 Model에 포함
- Model은 View와 직접적으로 연결되어 있지 않고 Controller를 통해서 데이터를 전달

```swift
// Model
struct Product {
    var category: String
    var name: String
    var price: Int
    var size: String
    var image: String
}
```

쇼핑몰로 다시 예를 들어보면,
상의 카테고리에 들어갔을 때 필요한 데이터, 변수들을 관리하는 역할을 합니다.

해당 카테고리 이름과 상품 이름, 가격, 사이즈 등등의 데이터를 보여줄 수 있으니까 이러한 변수들을 관리합니다.

View와 독립되어 있기 때문에 View의 요소와는 전혀 상관이 없습니다.

상품 사진의 크기, 이름과 가격의 폰트 사이즈 등등
화면을 구성하는 것은 View의 담당이고 Model은 데이터 자체에 대한 정보만 다룹니다.

### Controller

- 앱의 핵심 로직을 가짐
- View, Model에 연결되어 중간 다리 역할로 View에게 보일 데이터들을 Model에서 가져옴

View를 통해서 화면에 어떻게 표현할지에 대한 로직을 다룹니다.

```swift
class ProductListViewController: UIViewController {
    @IBOutlet weak var productNameLabel: UILabel!
    @IBOutlet weak var productPriceLabel: UILabel!
    @IBOutlet weak var buyButton: UIButton!

    var productModel: Product!
    
    override func viewDidLoad() {
        super.viewDidLoad()
            
        // productModel 데이터 받아오기
        getProductList()
        
        // 실제 앱을 만들 때는 상품 데이터가 여러개니까
        // 이렇게 하나하나 넣지 않고,
        // TableView 같은 거에 Model 데이터 리스트를 한 번에 넣어서 화면에 출력하겠죠!
        // Model 객체가 쓰이는 예시로만 봐주세요 !
        productNameLabel.text = productModel.name
        productPriceLabel.text = productModel.price.description

    }

    func getProductList() {
        // 서버(DB) 통신으로 data 받아옴
        productModel = // 코드 생략
    }

    @IBAction func buyButtonAction(_ sender: Any) {
    }
    
}
```

Model 객체를 생성하고 상품 목록들을 가져와서 View가 화면을 잘 그리게끔 해줍니다.

화면을 보는 사용자는 화면을 통해서 새로운 요청을 보냅니다. ⇒ 액션
이런 요청도 Controller가 받아서 얻어야 되는 데이터를 Model에게 알려주고 Model에게서 받아온 데이터를 다시 View에게 알려줍니다.

### View

- 유저들이 볼 수 있는 UI
- 유저들에게 데이터를 보여주고, 화면을 구성하는 코드들 포함
- Controller로부터 데이터를 받아 화면에 보여주는 역할

스토리보드를 비롯한 인터페이스 요소들이며 사용자가 직접 볼 수 있는 영역입니다.
(UIButton, UIImage, UILabel, .. 등등)

View에서 생기는 변화들은 Delegate와 DataSource를 사용하여 Controller가 감지하게 됩니다.

## 애플에서의 MVC 패턴

MVC 패턴은 UIKit에서 채택하기로 한 디자인 패턴이기 때문에 권장하기도 하고 일반적으로 사용하기도 합니다. 물론 다른 아키텍쳐를 채택해서 사용할 수 있습니다!

앱의 규모가 작을 때는 빠르고 간단하게 적용할 수 있는 패턴이지만, ViewController의 특성상 앱의 규모가 크고 기능이 많을 때에는 코드의 양이 많아져서 유지보수에 불편함을 느낄 수 있어서 다른 패턴을 적용하는 것도 좋은 방법이 될 수 있습니다.
