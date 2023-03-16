# Delegate란 무엇인지 설명하고, retain 되는지 안되는지 그 이유를 함께 설명하시오.

1. 프로토콜 이란?
    1. wift에서 delegate는 객체 지향 프로그래밍에서 자주 사용되는 디자인 패턴 중 하나입니다. Delegate는 객체가 다른 객체에게 일부 기능을 위임할 수 있는 방법을 제공합니다. 이를 통해 객체 간의 결합도를 낮출 수 있으며, 유연성과 재사용성을 높일 수 있습니다.
    Delegate 패턴에서, 먼저 프로토콜(Protocol)을 정의합니다. 이 프로토콜에는 delegate가 구현해야 하는 메서드나 프로퍼티가 포함됩니다. 그리고 이 프로토콜을 채택하는 객체는, 프로토콜에서 요구하는 **메서드와 프로퍼티를 모두 구현해야 합니다**. 이렇게 하면, 이 객체는 delegate 역할을 할 수 있게 됩니다.
    Swift에서는 delegate를 구현하기 위해 프로토콜을 정의하고, 클래스나 구조체가 이 프로토콜을 채택하도록 합니다. 예를 들어, UITableViewDataSource 프로토콜을 채택하는 객체는 UITableView의 데이터 소스(delegate) 역할을 할 수 있습니다.
    Delegate를 사용하면 **객체 간의 결합도를 낮추고, 코드의 재사용성과 유연성을 높일 수 있습니다.** 이를 통해 코드의 유지보수성도 향상시킬 수 있습니다.
2. 예제로 보는 Delegate
    1. Swift에서 delegate를 구현하는 방법에는 여러 가지가 있지만, 대표적인 방법은 프로토콜을 정의하고, 해당 프로토콜을 채택하도록 하는 것입니다. 아래는 간단한 예제 코드입니다.

```swift
//예제 소스코드

protocol MyDelegate: class {
    func didTapButton()
}

// 델리게이트를 사용하는 클래스
class MyClass {
    weak var delegate: MyDelegate?
    
    func someMethod() {
        // 델리게이트 메서드 호출
        delegate?.didTapButton()
    }
}

// 델리게이트를 구현하는 클래스
class MyViewController: UIViewController, MyDelegate {
    let myClass = MyClass()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 델리게이트 설정
        myClass.delegate = self
    }
    
    // 델리게이트 메서드 구현
    func didTapButton() {
        print("Button tapped")
    }
}
```

1. 위 코드에서 MyClass는 MyDelegate 프로토콜을 채택한 델리게이트 객체를 가지고 있습니다. MyClass의 someMethod()에서는 delegate를 통해 델리게이트 메서드인 didTapButton()을 호출합니다.
MyViewController는 MyDelegate를 구현하고 있으며, viewDidLoad()에서 MyClass의 delegate를 self로 설정합니다. 이제 MyClass에서 delegate를 통해 didTapButton()을 호출하면, MyViewController의 didTapButton() 메서드가 실행됩니다

2. retain 되는지 안되는지 그 이유?
Swift에서 delegate 패턴을 구현할 때, delegate 속성을 weak으로 선언해야 하는 이유는 retain cycle을 방지하기 위해서입니다.
만약 delegate를 strong으로 선언하면, delegate 객체는 MyClass나 MyViewController 객체와 같이 서로를 강한 참조하게 됩니다. 
이 때, MyClass와 MyViewController가 서로 참조하면서 메모리 누수가 발생할 수 있습니다. 이를 retain cycle이라고 합니다. 
하지만 delegate를 weak으로 선언하면, MyClass나 MyViewController 객체가 delegate를 참조하는 것과 달리, delegate가 MyClass나 MyViewController 객체를 참조하지 않습니다. 이는 retain cycle이 발생하지 않도록 해줍니다.
따라서, Swift에서 delegate 패턴을 구현할 때는 delegate 속성을 weak으로 선언해주어야 메모리 누수를 방지할 수 있습니다.
다시 한 번 위의 예제를 가져오겠습니다.
    
    

```swift
//예제 소스코드

protocol MyDelegate: class {
    func didTapButton()
}

// 델리게이트를 사용하는 클래스
class MyClass {
		//!weak 설정
    weak var delegate: MyDelegate?
    
    func someMethod() {
        // 델리게이트 메서드 호출
        delegate?.didTapButton()
    }
}

// 델리게이트를 구현하는 클래스
class MyViewController: UIViewController, MyDelegate {
    let myClass = MyClass()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 델리게이트 설정
        myClass.delegate = self
    }
    
    // 델리게이트 메서드 구현
    func didTapButton() {
        print("Button tapped")
    }
}
```

1. 위 코드에서 MyClass는 MyDelegate 프로토콜을 채택한 델리게이트 객체를 가지고 있습니다. 이 때, MyClass의 delegate 속성은 weak으로 선언되어 있습니다.
MyViewController는 MyDelegate를 구현하고 있으며, MyClass 객체의 delegate 속성을 자신으로 설정합니다. 이 때, MyViewController 객체는 MyClass 객체를 강한 참조하지 않습니다. 즉, MyViewController 객체와 MyClass 객체 간의 참조는 unowned 상태입니다.
이렇게 하면, MyClass와 MyViewController가 서로 참조하지 않기 때문에 retain cycle이 발생하지 않습니다. 또한, MyClass와 MyViewController가 모두 해제될 때 deinit 메서드가 호출되어 메모리 누수를 방지할 수 있습니다.
