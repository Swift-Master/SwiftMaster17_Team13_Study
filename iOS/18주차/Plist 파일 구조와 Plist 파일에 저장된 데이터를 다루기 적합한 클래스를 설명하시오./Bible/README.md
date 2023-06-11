# Plist 파일 구조와 Plist 파일에 저장된 데이터를 다루기 적합한 클래스를 설명하시오.

Property List라고도 하는 PList는 XML을 사용하여 `Key-Value 딕셔너리 형태의 계층적인 데이터 구조`를 가진 파일(.plist 확장자)입니다.  
소량의 데이터를 저장하는데에 적합해서 주로 설정 파일, 사용자 환경 설정 등과 같은 데이터를 다룹니다.

Plist 파일에는 String, Array, Dictionary, Bool, Date 등등 Swift에서 사용하는 대부분의 타입을 사용할 수 있습니다.

XCode에서 프로젝트를 생성하면 자동으로 Info.plist가 있는 걸 볼 수 있고 여기에는 앱의 기본 정보들이 들어있습니다.  
새로운 PList를 만들어서 사용해봅시다.

새로운 PList 파일에는 맥북 정보를 담았습니다.  
<img width="600" alt="사진4" src="https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/59015538/43a1970c-b63b-4bf3-8ffd-9bc83897f2a9">

XCode의 인터페이스로 항목을 추가하거나 수정할 수 있고, 파일 우클릭 -> Open as -> Source Code로 들어가서 XML 형태로도 파일을 사용할 수 있습니다.

이제 이 파일 데이터를 읽어봅시다.
```swift
guard let plistPath = Bundle.main.path(forResource: "StudyPropertyList", ofType: "plist") else {
    print("해당 파일 없음")
    return
}

guard let plistData = FileManager.default.contents(atPath: plistPath) else {
    print("해당 파일 읽기 실패")
    return
}

var format = PropertyListSerialization.PropertyListFormat.xml
do {
    let plistDictionary = try PropertyListSerialization.propertyList(from: plistData, options: [], format: &format) as? [String: Any] // PList 목록 반환
    
    if let dictionary = plistDictionary {
        print(dictionary)
        print(dictionary["Name"]!)
        print(dictionary["OS Version"]!)
    }
    
} catch {
    print("ERROR")
}
```
<img width="700" alt="사진6" src="https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/59015538/9ed360e9-84a1-43d9-be5b-144ca6bad20b">  
콘솔에 StudyPropertyList를 Dictionary에 담아 잘 출력되는 것을 볼 수 있습니다.

Plist의 목록을 가져오는 `propertyList()` 메서드는,  
```swift
class func propertyList(
    from data: Data,
    options opt: PropertyListSerialization.ReadOptions = [],
    format: UnsafeMutablePointer<PropertyListSerialization.PropertyListFormat>?
) throws -> Any
```
오류를 던지는 함수여서 try-catch문과 함께 작성해주어야 합니다.

이 메서드는 `PropertyListSerialization` 클래스 내부에 구현되어 있어서 PList 파일을 불러오는데에 사용됩니다.

`PropertyListSerialization` 클래스 이외에도 `PropertyListEncoder`, `PropertyListDecoder` 클래스를 사용하여 PList의 데이터를 사용할 수 있습니다.

## 출처
[What is plist in iOS?](https://www.tutorialspoint.com/what-is-plist-in-ios)
[PropertyListSerialization - Apple Developer](https://developer.apple.com/documentation/foundation/propertylistserialization)

