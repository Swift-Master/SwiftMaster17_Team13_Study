# Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명하시오.

iOS 개발을 공부하면 Cocoa 혹은 Cocoa Touch 프레임워크에 대해 한 번은 들어봤겠죠?!

먼저 Cocoa 프레임워크는 Foundation과 AppKit 프레임워크를 포함하고 있고 OS X 앱을 개발할 때 사용합니다. 즉 macOS 앱 개발용!  
Cocoa Touch 프레임워크는 Foundation과 UIKit 프레임워크를 포함하고 있고 iOS 앱을 개발할 때 사용합니다.  
Cocoa, Cocoa Touch는 이 외에도 다양한 프레임워크를 포함하고 있는 프레임워크입니다!

Foundation Kit은 곧 Foundation 프레임워크라고도 하는데요. 
OS X나 iOS뿐만 아니라 watchOS, tvOS까지 애플의 다양한 플랫폼 앱 개발을 위해서는 꼭 필요한 프레임워크입니다.

Foundation은 문자, 문자열, 숫자와 같은 기본적인 타입 외에. 
배열, 딕셔너리, 집합과 같은 콜렉션들과 날짜와 시간, 파일 처리, 네트워킹 등 정말 일반적인 작업을 위한 기능들을 제공합니다.

```swift
import Combine
import CoreFoundation
import Darwin
import Darwin.uuid
import Dispatch
import Foundation.FoundationErrors
import Foundation.FoundationLegacySwiftCompatibility
import Foundation.NSArray
import Foundation.NSAttributedString
import Foundation.NSAutoreleasePool
import Foundation.NSBundle
import Foundation.NSByteCountFormatter
import Foundation.NSByteOrder
import Foundation.NSCache
import Foundation.NSCalendar
import Foundation.NSCharacterSet
import Foundation.NSCoder
import Foundation.NSComparisonPredicate
import Foundation.NSCompoundPredicate
import Foundation.NSData
import Foundation.NSDate
import Foundation.NSDateComponentsFormatter
import Foundation.NSDateFormatter
import Foundation.NSDateInterval
import Foundation.NSDateIntervalFormatter
import Foundation.NSDecimal
import Foundation.NSDecimalNumber
import Foundation.NSDictionary
import Foundation.NSEnergyFormatter
import Foundation.NSEnumerator
import Foundation.NSError
import Foundation.NSException
import Foundation.NSExpression
import Foundation.NSExtensionContext
import Foundation.NSExtensionItem
import Foundation.NSExtensionRequestHandling
import Foundation.NSFileCoordinator
import Foundation.NSFileHandle
import Foundation.NSFileManager
import Foundation.NSFilePresenter
import Foundation.NSFileVersion
import Foundation.NSFileWrapper
import Foundation.NSFormatter
import Foundation.NSHTTPCookie
import Foundation.NSHTTPCookieStorage
import Foundation.NSHashTable
import Foundation.NSISO8601DateFormatter
import Foundation.NSIndexPath
import Foundation.NSIndexSet
import Foundation.NSInflectionRule
import Foundation.NSInvocation
import Foundation.NSItemProvider
import Foundation.NSJSONSerialization
import Foundation.NSKeyValueCoding
import Foundation.NSKeyValueObserving
import Foundation.NSKeyedArchiver
import Foundation.NSLengthFormatter
import Foundation.NSLinguisticTagger
import Foundation.NSListFormatter
import Foundation.NSLocale
import Foundation.NSLock
import Foundation.NSMapTable
import Foundation.NSMassFormatter
import Foundation.NSMeasurement
import Foundation.NSMeasurementFormatter
import Foundation.NSMetadata
import Foundation.NSMetadataAttributes
import Foundation.NSMethodSignature
import Foundation.NSMorphology
import Foundation.NSNetServices
import Foundation.NSNotification
import Foundation.NSNotificationQueue
import Foundation.NSNull
import Foundation.NSNumberFormatter
import Foundation.NSObjCRuntime
import Foundation.NSObject
import Foundation.NSOperation
import Foundation.NSOrderedCollectionChange
import Foundation.NSOrderedCollectionDifference
import Foundation.NSOrderedSet
import Foundation.NSOrthography
import Foundation.NSPathUtilities
import Foundation.NSPersonNameComponents
import Foundation.NSPersonNameComponentsFormatter
import Foundation.NSPointerArray
import Foundation.NSPointerFunctions
import Foundation.NSPort
import Foundation.NSPredicate
import Foundation.NSProcessInfo
import Foundation.NSProgress
import Foundation.NSPropertyList
import Foundation.NSProxy
import Foundation.NSRange
import Foundation.NSRegularExpression
import Foundation.NSRelativeDateTimeFormatter
import Foundation.NSRunLoop
import Foundation.NSScanner
import Foundation.NSSet
import Foundation.NSSortDescriptor
import Foundation.NSStream
import Foundation.NSString
import Foundation.NSTextCheckingResult
import Foundation.NSThread
import Foundation.NSTimeZone
import Foundation.NSTimer
import Foundation.NSURL
import Foundation.NSURLAuthenticationChallenge
import Foundation.NSURLCache
import Foundation.NSURLConnection
import Foundation.NSURLCredential
import Foundation.NSURLCredentialStorage
import Foundation.NSURLError
import Foundation.NSURLProtectionSpace
import Foundation.NSURLProtocol
import Foundation.NSURLRequest
import Foundation.NSURLResponse
import Foundation.NSURLSession
import Foundation.NSUUID
import Foundation.NSUbiquitousKeyValueStore
import Foundation.NSUndoManager
import Foundation.NSUnit
import Foundation.NSUserActivity
import Foundation.NSUserDefaults
import Foundation.NSValue
import Foundation.NSValueTransformer
import Foundation.NSXMLParser
import Foundation.NSXPCConnection
import Foundation.NSZone
import ObjectiveC
import _Concurrency
import _StringProcessing
```
Foundation이 정의된 곳으로 가보면 거의 대부분 NS로 시작하는 클래스들을 포함하고 있는 것을 볼 수 있습니다.  
Foundation은 코코아, 코코아 터치 프레임워크의 필수 요소이며 Objective-C 기반으로 만들어졌기 때문입니다.  
또한 코코아, 코코아 터치 프레임워크의 최상위 루트 클래스가 NSObject입니다.

```swift
import Foundation

var str = "Hello"
print(str)

var str2: NSString = "Hello2"
print(str2)

/*
Hello
Hello2

정상출력
*/
```

```swift
//import Foundation

var str = "Hello"
print(str)

var str2: NSString = "Hello2" // ERROR ! Cannot find type 'NSString' in scope
print(str2)
```
import Foundation을 주석처리해보면,  
str2에서는 NSString을 찾을 수 없어서 에러가 나고 import Foundation을 추가하라고 해줍니다.

```swift
//import Foundation

var str = "Hello"
print(str)

/*
Hello

정상출력
*/
```
import Foundation이 없어도 str은 그대로 출력이 잘 됩니다.  
str은 NSString이 아닌 Swift의 String이기 때문입니다.

Swift의 String은 Swift Standard Library로 Swift 파일에서 별도 라이브러리나 프레임워크를 import하지 않아도 사용할 수 있습니다.  
(Int, Double, Bool, String, Array, Optional 등등... ~~~ 아주아주 기본적인 문법 대부분!)

결론적으로,  
Foundation Kit은 Cocoa, Cocoa Touch Freamework의 요소 중 하나이며 Objective-C 기반으로 작성되었고 다양한 데이터 타입과 클래스, 함수, 프로토콜 등을 제공하는 프레임워크입니다.
- NSString, NSMutableString 클래스 : 문자열 처리를 위한 다양한 메서드 사용 가능
- NSDate, NSDateComponents 클래스 : 날짜와 시간 연산, 포맷 변환, 타임존 처리 등 기능 수행
- NSFileManager 클래스 : 파일 및 디렉토리 생성, 삭제, 이동의 작업 수행
- NSURLSession, NSURLRequest 클래스 : 네트워크 연결 및 데이터 송수신과 관련된 기능 제공

이 외에도 다양한 클래스와 기능을 제공하고 있고,  
Swift로 개발을 하면서 NSObject를 상속받는 클래스의 기능이 필요할 때 import Foundation하여 사용하면 될 것 같습니다.  
(참고로 UIKit 내부에 import Foundation이 되어있어서 import UIKit을 한 상태에서는 import Foundation을 따로 하지 않아도 됩니다!)
