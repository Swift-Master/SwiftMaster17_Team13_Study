# NSCache와 딕셔너리로 캐시를 구성했을때의 차이를 설명하시오.
## NSCache란 뭘까

- 리소스가 부족할 때 제거될 수 있는 임시 키-값 쌍을 임시로 저장하는 데 사용하는 변경 가능한 컬렉션입니다.

## 딕셔너리로 캐시를 구성할 때와의 차이점

### 자동 제거 기능
- 캐시가 시스템 메모리를 너무 많이 사용하지 않도록 하는 다양한 자동 제거 로직이 존재합니다. 다른 애플리케이션에 메모리가 필요한 경우 캐시에서 일부 항목을 제거하여 메모리 사용 공간을 최소화합니다.

- 이 때문에 일반적으로  생성 비용이 큰 임시 데이터로 구성된 객체를  임시로 저장할 때 사용합니다. 저장된 객체를 재사용함으로써 성능상의 이점을 얻을 수 있으나, 중요도가 떨어지거나 메모리가 부족한 경우 임의로 삭제될 수 있습니다.

- 자동 삭제 기능은 NSDiscardableContent 프로토콜 채택을 통해 임의로 기능 변경이 가능합니다. NSDiscardableContent 객체가 캐시에 저장되면 캐시는 제거 시 discardContentIfPossible()를 호출하여 객체를 NSDiscardableContent 객체를 제거합니다.

### Thread-Safe
- NSCache는 Thread - Safe한 객체이기 때문에 캐시를 Lock(작업 중인 쓰레드 외에 다른 쓰레드의 접근을 허용하지 않는 것)하지 않아도 다른 스레드에서 캐시의 항목에 접근하여 수정/삭제가 가능합니다.

### NSMutableDictionary 객체와 달리 캐시에 넣은 키 객체 복사 ❌
- [NSMutableDictionary와 NSCache간 키 처리 방식](https://stackoverflow.com/questions/69377994/does-the-different-way-of-handling-key-between-nsmutabledictionary-and-nscache)
- 아래처럼 딕셔너리의 경우 구조체처럼 원본 키 객체를 참조하지 않고 새 객체를 생성합니다.
	- dicKey가 "key" -> "changedKey"로 변경되었으나 변경 전 key-value 쌍이 딕셔너리에 존재
```swift
let mutableDic = NSMutableDictionary()
var dicKey: NSString = "key"
mutableDic.setObject("one", forKey: dicKey)
dicKey = "changedKey"
mutableDic.setObject("two", forKey: dicKey)

print(mutableDic.object(forKey: "key") ?? "") //"one"
print(mutableDic.object(forKey: "changedKey") ?? "") //"two"
```

- NSCache 객체는 클래스처럼 원본 키 객체를 참조하기 때문에  원본 키 객체의 변경을 따라갑니다.
	- "key" - "one"의 key-value쌍이 객체에 존재
	- "changedKey"로 변경 후 "changedKey" - "one" key value 쌍이 객체에 존재
	- "changedKey" key의 value를 "two"로 변경하여 최종적으로 "changedKey" - "two"만 남게됨
```swift
let cache = NSCache<NSString, NSString>()
var cacheKey: NSString = "key"
cache.setObject("one", forKey: cacheKey)
cacheKey = "changedKey"
cache.setObject("two", forKey: cacheKey)

print(cache.object(forKey: "key") ?? "") //""
print(cache.object(forKey: "changedKey") ?? "") //"two"
```



## 참고 문헌
- [NSCache와 NSDictionary의 차이점](https://inuplace.tistory.com/1050)
- [애플 공식 문서 - NSCache](https://developer.apple.com/documentation/foundation/nscache)