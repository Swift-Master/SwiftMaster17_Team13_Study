# Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.

## Delegate 패턴이란?
### 정의
- Delegate의 사전적 의미 : 대리자, 대상에게 역할을 위임하다
- 객체 지향 디자인 패턴으로서 코드의 재사용성이 증가하고 정보 은닉이 가능합니다.

### 구현 방법
- 객체의 역할을 먼저 구분한다.
  - 데이터를 전달받고 싶은 객체는 delegate/receiver
  - 데이터를 전달하게될 객체는 sender
- sender 객체는 내부에 채택한 delegate프로토콜 타입의 변수(관습적으로 delegate라는 이름)를 가지고 객체 내부에 구현된 메서드에서 해당 delegate의 메서드를 호출합니다.

```
struct sender {
  var delegate : ExDelegate? //보통 옵셔널로 선언
  
  var doSomething() {
    delegate?.protocolImplementMethod() 
    // 대리자가 존재한다면, 그 대리자가 구현해놓은 프로토콜 메서드를 호출
  }
}
```
</br>

- receiver 객체는 sender 객체의 delegate가 자신임을 먼저 알려야 합니다. 또, sender에서 프로토콜 메서드를 사용할 수 있도록 프로토콜을 채택 후, 구현해줍니다.

```
struct receiver {

 sender.delegate = self 
// 국어 문법대로 이해하자. -> sender 객체의 대리자는 self(receiver객체)이다.

}

extension receiver : ExDelegate { 
// 보통 가시성과 역할 분리를 위해 extension에서 프로토콜 채택 후 메서드를 구현해줍니다.

  protocolImplementMethod() {
    //구현
  }
}
```

### 

## 예시(Swift 공식 가이드북)

### 커스텀 프로토콜 정의

- AnyObject는 모든 class가 사용 가능하도록 채택한다는 의미입니다.
```
protocol DiceGame {
  var dice: Dice { get }
  func play()
}

protocol DiceGameDelegate: AnyObject {
  func gameDidStart(_ game: DiceGame)
  func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
  func gameDidEnd(_ game: DiceGame)
}
```

### sender 객체 구현

- 내부에 delegate 변수와 옵셔널 체이닝 형태로 프로토콜의 메서드를 호출하고 있습니다.

```
class SnakesAndLadders: DiceGame {
  let finalSquare = 25
  let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
  var square = 0
  var board: [Int]
  init() {
    board = Array(repeating: 0, count: finalSquare + 1)
    board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
    board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
  }
  weak var delegate: DiceGameDelegate?
  func play() {
    square = 0
    delegate?.gameDidStart(self)
    gameLoop: while square != finalSquare {
      let diceRoll = dice.roll()
      delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)
      switch square + diceRoll {
      case finalSquare:
        break gameLoop
      case let newSquare where newSquare > finalSquare:
        continue gameLoop
      default:
        square += diceRoll
        square += board[square]
      }
    }
    delegate?.gameDidEnd(self)
  }
}
```
### receiver(delegate) 객체 구현


- 커스텀 프로토콜을 채택해서 내부 메서드들을 구현해주고 있습니다.

```
class DiceGameTracker: DiceGameDelegate {
  var numberOfTurns = 0
  func gameDidStart(_ game: DiceGame) {
    numberOfTurns = 0
    if game is SnakesAndLadders {
      print("Started a new game of Snakes and Ladders")
    }
    print("The game is using a \(game.dice.sides)-sided dice")
  }
  func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
    numberOfTurns += 1
    print("Rolled a \(diceRoll)")
  }
  func gameDidEnd(_ game: DiceGame) {
    print("The game lasted for \(numberOfTurns) turns")
  }
}
```
### 객체간의 연결!

- 맥 앱을 통해 만들어진 코드이기 때문에 이렇게 특별히 외부에서 인스턴스를 두 개 생성 후 DiceGameTracker의 객체 tracker를 SnakesAndLadders의 객체 game의 대리자로 설정해주었습니다. 
- 이를 통해 tracker의 play() 메서드가 호출되는 순간, 대리자인 game도 호출되어 구현한 메서드로 동작하게 됩니다. game이 tracker의 일을 대신해주는 셈이죠.

```
let tracker = DiceGameTracker()
let game = SnakesAndLadders()
game.delegate = tracker
game.play()
// Started a new game of Snakes and Ladders (새로운 뱀과 사다리 게임을 시작함)
// The game is using a 6-sided dice (6-면체 주사위를 사용하는 게임임)
// Rolled a 3 (3이 나옴)
// Rolled a 5 (5가 나옴)
// Rolled a 4 (4가 나옴)
// Rolled a 5 (5가 나옴)
// The game lasted for 4 turns (게임을 4턴 동안 지속함)
```


## UIKit 내부에서의 delegate 패턴 동작

### 동작과정은 다르지 않다 (ex : textField)
sender 객체(textField)와의 상호작용(터치, 값변경 등..) 

 -> textField 객체 내부의 메서드에서 delegate변수의 옵셔널 체이닝을 통해 TextFieldDelegate프로토콜 메서드 호출

 -> delegate(receiver)로 설정되어 있는 객체(주로 viewController)가 호출됨. 해당 객체는 TextFieldDelegate를 채택하여 이런 호출을 받았을 때 어떤 동작을 할지 구현해놓았음.

 -> textField의 value값 길이 제한, 숫자 입력 금지, 전체 삭제 활성화 등 다양한 동작 가능..  

### 허나.. 한가지 다른 점

- sender에서 delegate를 호출하는 코드는 사실 definition에 들어가도 볼 수 없다!
- 왜? 애플의 기업비밀...

```
class UITextField {
  ...
  func callDelegate() { // 실제이름은 callDelegate가 아닐것이다.
    ...
      delegate?.프로토콜메서드() 
  }

}
```

## 참고 자료 및  읽을거리

### [애플 공식문서](https://developer.apple.com/documentation/swift/using-delegates-to-customize-object-behavior)

### [Swift 공식 가이드북 번역- protocol부분](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html#ID278)
