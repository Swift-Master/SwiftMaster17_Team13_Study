# Swift Standard Library의 map, filter, reduce, compactMap, flatMap에 대하여 설명하시오.

map, filter, reduce, compactMap, flatMap은 고차함수의 종류들입니다.

고차함수(Higher-order Function)는 간단하게 말해서 다른 함수를 인자로 받거나 함수의 실행 결과를 함수로 반환하는 함수입니다.

이 고차함수는 일급객체(혹은 일급시민)로 함수의 매개변수로 전달이 가능하고, 함수의 반환값이 될 수 있어야 하고, 변수에 저장이 가능해야 되는 조건을 만족합니다.
→ 일급객체, 일급시민은 우리가 자연스럽게 썼던 대부분의 변수들을 생각하면 됨!

고차함수는 특히 컬렉션(배열, 리스트, 집합 등등)을 다룰 때 자주 사용합니다.

## map

```swift
func map<T>(_ transform: (Self.Element) throws -> T) rethrows -> [T]
```

map은 매개 변수로 전달받은 함수를 실행해서 그 결과값을 반환해줍니다.

예를 들어, 문자열이 담긴 배열 각 요소들의 길이를 새로운 배열에 담아봅시다.

```swift
let numbers = ["1", "22", "333", "4444", "55555"]
var countNumbers = [Int]()
for i in numbers {
    countNumbers.append(i.count)
}
print(countNumbers) // [1, 2, 3, 4, 5]
```

다른 방법도 있겠지만 보통 이렇게 for문으로 numbers 배열의 요소들에 접근하고 새로운 배열에 추가하는 방식으로 작성하게 되겠죠

map을 사용해봅시다.

```swift
let numbers = ["1", "22", "333", "4444", "55555"]
let countNumbers = numbers.map{ $0.count }
print(countNumbers) // [1, 2, 3, 4, 5]
```

$0은 for문의 i 변수처럼 numbers 요소들에 접근하게 됩니다.
코드가 엄청 간결해졌죠!

예를 하나 더 들어서 이번에는 정석적인 클로져의 형태로 사용해봅시다.

```swift
let numbers = [1, 2, 3, 4, 5]
let addNumbers = numbers.map { element in
    return element + 1
}
print(addNumbers) // [2, 3, 4, 5, 6]
```

element는 for문에서의 i와 $0와 같이 numbers 요소들에 접근합니다.
numbers 배열 값들에 1씩 더한 새로운 배열을 아주 간결하게 만들 수 있습니다.

어느정도 감이 잡히시나요!
map은 요소들에 접근해서 값을 변형한 새로운 변수(위의 예제에서 새로운 배열 변수)를 만들어 낼 때 유용하게 사용할 수 있습니다.

기능적으로 for-in 구문과 상당히 비슷하고 차이가 크지 않지만, map을 사용하면 코드가 더 간결하고 컴파일러 성능면에서 조금 더 우세한 부분이 있습니다.

## filter

```swift
func filter(_ isIncluded: (Self.Element) throws -> Bool) rethrows -> [Self.Element]
```

filter는 주어진 조건을 만족시킨 결과를 반환합니다.

매개 변수가 Bool로 반환되게 작성되어 있는데, 매개 변수로 전달할 클로져의 결과값은 참, 거짓으로 나오는 조건문이어야 됩니다. 

접근한 요소가 조건을 만족하는지 참 거짓을 판별해서 만족하는 결과값만 모아주게 됩니다.

```swift
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

let evenNumbers = numbers.filter{ $0 % 2 == 0 }
print(evenNumbers) // [2, 4, 6, 8, 10]

let oddNumbers = numbers.filter{ $0 % 2 != 0 }
print(oddNumbers) // [1, 3, 5, 7, 9]
```

1부터 10까지의 수들을 담은 numbers 배열에 filter를 사용해서 각각 `$0 % 2 == 0`과 `$0 % 2 ≠ 0` 조건식을 넣고 짝수와 홀수 숫자들로 나눈 새로운 배열을 만들었습니다.

다시 정석적인 클로져 형태로 만들어보면,

```swift
let evenNumbers = numbers.filter { (number: Int) -> Bool in
    return number % 2 == 0
}
print(evenNumbers)
```

이렇게 작성해도 똑같은 결과를 얻을 수 있습니다 !

```swift
let strings = ["A", "BB", "CCC", "DDDD", "FFFFF"]
let longStr = strings.filter{ $0.count > 3 }
print(longStr) // ["DDDD", "FFFFF"]
```

이런 식으로 조건에 맞는 요소들을 가져와야 할 때 아주 간결하게 만들 수 있습니다.

filter를 쓰지 않았다면 for문과 if문을 섞어서 코드가 좀 더 복잡해보였겠죠!

하지만 성능적으로는 filter가 for문을 사용했을 때와 크게 차이가 나지는 않는 것 같습니다. 성능면에서 이점을 바라기 보다는 가독성이 주는 이점이 더 큰 방식입니다.

## reduce

```swift
func reduce<Result>(
    _ initialResult: Result,
    _ nextPartialResult: (Result, Self.Element) throws -> Result
) rethrows -> Result
```

reduce는 ‘줄이다’라는 사전적인 의미를 갖는데,
요소들을 결합, 누적해서 하나의 통합적인 결과를 반환합니다.

모든 값을 하나로 합치는데, 각 요소를 전달 받아 연산을 진행하면 다음 실행 때 이 값을 다시 사용하는 순환 형태를 가집니다.

`initialResult`는 초기 누적값으로 사용 할 값을 의미하고, 
`nextPartialResult`는 클로저로 두 개의 매개 변수를 받고 있는데 이 중 하나인 `Result`는 initialResult로 전달받은 초기값이거나 이전 순회에서의 최종 결과값을 의미합니다. `Element`는 현재 연산에서 사용 할 요소가 됩니다.

```swift
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let sumNumbers = numbers.reduce(0, { resultValue, nextValue in 
    return resultValue + nextValue
})
print(sumNumbers) // 55
```

초기값 0부터 시작해서 resultValue는 맨 처음 초기값부터 시작해서 연산을 완료한 누적된 값이 될 것이고, nextValue는 1부터 10까지의 연산을 할 값이 됩니다.

글로 보면 조금 복잡하기 때문에 이 더하기 연산이 어떻게 진행되는지 보면,

```swift
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let sumNumbers = numbers.reduce(0, { resultValue, nextValue in 
    print("resultValue : \(resultValue), nextValue : \(nextValue)")
    return resultValue + nextValue
})
print(sumNumbers)
/*
resultValue : 0, nextValue : 1
resultValue : 1, nextValue : 2
resultValue : 3, nextValue : 3
resultValue : 6, nextValue : 4
resultValue : 10, nextValue : 5
resultValue : 15, nextValue : 6
resultValue : 21, nextValue : 7
resultValue : 28, nextValue : 8
resultValue : 36, nextValue : 9
resultValue : 45, nextValue : 10
55
*/
```

이렇게 resultValue에는 누적값이 쌓이고 nextValue에는 누적에 사용 할 값이 담기는 것을 볼 수 있고, 마지막 연산 결과(55)는 반환값으로 받게 됩니다.

```swift
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let sumNumbers = numbers.reduce(5, { resultValue, nextValue in 
    print("resultValue : \(resultValue), nextValue : \(nextValue)")
    return resultValue + nextValue
})
print(sumNumbers)
/*
resultValue : 5, nextValue : 1
resultValue : 6, nextValue : 2
resultValue : 8, nextValue : 3
resultValue : 11, nextValue : 4
resultValue : 15, nextValue : 5
resultValue : 20, nextValue : 6
resultValue : 26, nextValue : 7
resultValue : 33, nextValue : 8
resultValue : 41, nextValue : 9
resultValue : 50, nextValue : 10
60
*/
```

0으로 설정했던 초기값을 다른 값으로 하면 이런 결과도 얻을 수 있습니다.

원리 파악이 조금 됐으니 이제 단축해서 다시 봐봅시다.

```swift
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let sumNumbers = numbers.reduce(0) { $0 + $1 }
print(sumNumbers) // 55
```

$0은 위에서의 resultValue, $1은 nextValue와 같은 역할을 합니다.

$0, $1, $2 … 등등 클로져에서 사용하는 인자들을 나타내는 표현식입니다.

map이나 filter에서는 하나의 값만 사용했기 때문에 $0만 쓰였는데 reduce에서는 두 개의 값을 가지고 코드를 작성하기 때문에 0 다음 수인 $1도 쓰이게 됐습니다.

## compactMap

```swift
func compactMap<ElementOfResult>(_ transform: (Self.Element) throws -> ElementOfResult?) rethrows -> [ElementOfResult]
```

각 요소로 연산을 진행할 때 nil이 아닌 결과를 반환해주는 map입니다.
아까 위에서의 map과의 차이점이 있다면 이 compactMap은 옵셔널로 정의된 것을 볼 수 있습니다.

예제로 어떤 차이점이 있는지 봅시다.

```swift
let possibleNumbers = ["1", "2", "three", "///4///", "5"]

let mapped = possibleNumbers.map { str in Int(str) }
print(mapped) // [Optional(1), Optional(2), nil, nil, Optional(5)]

let compactMapped = possibleNumbers.compactMap { str in Int(str) }
print(compactMapped) // [1, 2, 5]
```

possibleNumbers에 있는 요소들을 Int로 형변환한 값들을 담아 새로운 배열로 만든 코드입니다.

일반 map을 사용하게 되면 “three”와 “///4///”은 Int로 형변환이 불가능하기 때문에 nil이 들어가게 됩니다.

compactMap을 사용한 경우 이러한 nil은 들어가지 않고 잘 변환된 값만 담아서 반환해주었습니다.

compactMap은 nil 값을 포함하지 않고 반환해주기 때문에 nil이 들어갈 수 있는 상황에서 nil값을 받고 싶지 않을 때 사용하면 좋을 것 같습니다!

## flatMap

```swift
func flatMap<SegmentOfResult>(_ transform: (Self.Element) throws -> SegmentOfResult) rethrows -> [SegmentOfResult.Element] where SegmentOfResult : Sequence
```

2차원 배열의 형태를 1차원 배열로 변환해서 결과값을 보내줍니다.
2차원에서 1차원으로 flattened된…! 이런 느낌

```swift
let numbers = [1, 2, 3, 4]

let mapped = numbers.map { Array(repeating: $0, count: $0) }
print(mapped) // [[1], [2, 2], [3, 3, 3], [4, 4, 4, 4]]

let flatMapped = numbers.flatMap { Array(repeating: $0, count: $0) }
print(flatMapped) // [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
```

Array(repeating: $0, count: $0)은 $0 요소를 $0번 만큼 반복해서 새로운 배열로 만들어줍니다.

map을 사용하면 각 요소들의 결과값인 배열을 큰 하나의 배열에 포함하여 2차원 배열을 반환했습니다.

flatMap을 사용하면 map과 다르게 하나의 1차원 배열로 모든 요소들을 합쳐서 결과값을 보여줍니다.

```swift
let numbers3 = [[[1], [2, 2], [3, 3, 3]]]

let flatMapped3 = numbers3.flatMap{ $0 }
print(flatMapped3) // [[1], [2, 2], [3, 3, 3]]

let flatMapped = flatMapped3.flatMap{ $0 }
print(flatMapped) // [1, 2, 2, 3, 3, 3]
```

또한 3차원 배열의 경우 한 번 flatMap으로 변환 시켜주면 2차원 배열이 되고, 한 번 더 변환 시켜주면 1차원 배열을 반환해줍니다.

이렇게 여러 번 사용해야 되는 경우

```swift
let numbers3 = [[[1], [2, 2], [3, 3, 3]]]
let flatMapped3 = numbers3.flatMap{ $0 }.flatMap{ $0 }
print(flatMapped3) // [1, 2, 2, 3, 3, 3]
```

한 번에 두 번을 모두 작성해서 사용할 수 있습니다!

## 같이 사용하기

방금 flatMap을 한 번에 두 번 연달아 사용했던 것처럼 다른 고차함수들도 한 번에 여러 번 사용할 수 있습니다.

예를 들어, 숫자들을 짝수만 고르고 2배씩 곱한 배열을 만들어야 되면,

```swift
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
let resultArry = numbers.filter{ $0 % 2 == 0 }.map{ $0 * 2 }
print(resultArry) // [4, 8, 12, 16, 20]
```

이렇게 한 줄로 아주 간단하게 코드를 작성할 수 있습니다!

## 참고

[https://jeong9216.tistory.com/403](https://jeong9216.tistory.com/403)

위에서 설명한 함수들과 for-in 구문 속도 차이에 대한 글입니다!
한 번 가볍게 보는 것도 좋을 것 같습니다 :)
