# App Bundle의 구조와 역할에 대해 설명하시오.

## 번들이란?

- 번들은 코드와 리소스를 캡슐화하는 데 사용되는 macOS 및 iOS의 기본 기술입니다. 
- 개발자가 복합 바이너리 파일을 직접 생성하지 않고도 필요 리소스에 접근할 수 있는 경로를 제공해줍니다. 
- 언제든(개발 중, 배포 후..) 접근하여 수정할 수 있습니다.

### 번들의 장점

- 파일 시스템의 디렉토리 계층이기 때문에 파일만 포함하기 때문에 파일 기반 인터페이스를 통해 다른 유형의 파일을 열 때와 마찬가지로 번들 리소스를 열 수 있습니다. 이로 인해 Finder에서 간단히 번들을 드래그하여 설치, 재배치 및 제거할 수 있습니다.
    
- 디렉토리 구조를 통해 여러 현지화를 쉽게 지원할 수 있습니다.
    
- 여러 파일시스템(HFS, UFS)이나 CPU 아키텍쳐를 포함하기 때문에 그 시스템에만 존재하는 특장점을 활용가능합니다.
    
- 패키지이기도 하므로 중요한 리소스의 제거, 수정 또는 이름 변경과 같은 실수나 악의적 공격에 덜 취약합니다.
    
- 대부분의 코드(애플리케이션, 프레임워크(공유 라이브러리) 및 플러그인)를 번들로 묶을 수 있습니다. 정적 라이브러리, 동적 라이브러리, 셸 스크립트 및 UNIX 명령줄 도구는 번들 구조를 사용하지 않습니다.
    
- 로컬 시스템에 특별한 공유 라이브러리, 확장 및 리소스를 설치할 필요 없이 서버에서 직접 실행할 수 있습니다.

### 번들의 종류

- 애플리케이션(앱) 번들 : 개발자가 만드는 가장 일반적인 유형으로, 앱의 모든 것을 저장(코드 및 리소스)
- 프레임워크 번들 : 헤더 파일과 같은 동적 공유 라이브러리 및 관련 리소스를 관리
- 플러그인 및 Lodable 번들 : 앱의 동작을 동적으로 확장하는 방법 제공. `ios 미지원`

## 패키지 == Bundle?
<p align = "center"><image src = "https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/116094622/8443b91b-4413-4ac5-b9c8-59ac7df03cf0" width = 50% ></image></p>

### 패키지는 Finder가 단일 파일인 것처럼 제공되는 모든 디렉토리
- 앱 번들, Loadable 번들은 시스템에서 `불투명`(상세 파일에 접근할 수 없음)하게 취급되므로 패키지이지만, 프레임워크 번들의 경우 번들 자체는 단일 파일로 취급되나 그 디렉토리는 개발자가 헤더 및 기타 리소스에 접근할 수 있도록 **투명**하게 설정되어 있습니다. 
- 패키지는 사용자 경험을 개선하기 위해 존재하지만 번들은 개발자가 코드를 패키징하고 운영 체제가 해당 코드에 액세스하도록 돕는 데 더 적합합니다.


## 앱 번들

### 구조

- 디스크 공간을 절약하고 파일에 대한 액세스를 단순화하기 위해 외부 디렉토리가 거의 없는 비교적 평평한 구조를 사용합니다.
- 앱 실행 파일과 앱에서 사용하는 모든 리소스(앱 아이콘, 기타 이미지 및 현지화된 콘텐츠)는 최상위 번들 디렉토리에 위치합니다.  현지화 파일만 하위 디렉토리에 위치 가능하나 리소스 및 기타 관련 파일을 구성하기 위해  추가 하위 디렉터리를 만들 수 있습니다.
- 아래는 ios 앱의 구조 예시입니다.
```bash
MyApp.app
   MyApp
   MyAppIcon.png
   MySearchIcon.png
   Info.plist
   Default.png
   MainWindow.nib
   Settings.bundle
   MySettingsIcon.png
   iTunesArtwork
   en.lproj
      MyImage.png
   fr.lproj
      MyImage.png
```

 - MyApp.app : 실행파일(필수)
 - Info.plist : 번들 ID, 버전 번호 및 표시 이름과 같은 애플리케이션에 대한 구성 정보가 포함된 필수 파일
- 리소스 파일(권장)
	- Icon.png파일들: 홈화면, 검색 결과, 설정 등에 대한 아이콘
	- 시작이미지(Default.png)
	- MainWindow.nib : 앱이 실행되는 메인 윈도우 객체가 포함되어 있는 nib파일
- 기타 지원 파일
	- Settings.bundle : 기본 설정을 위한 plist 및 기타 리소스 파일 포함
	- 사용자 지정 리소스 파일(en.lproj, fr.lproj) : 현지화되지 않은 리소스는 최상위에, 현지화 리소스는 언어별 하위 디렉터리에 배치됩니다. nib, 이미지, 사운드파일, 설정 파일, 문자열, 커스텀 데이터 파일 등으로 구성되어 있습니다.

### 구성 항목 상세

사용 방식은 ios, macOS 모두 동일하나 구조나 위치, 사용여부는 다를 수 있습니다.

- `Info.plist`파일(필수)
	- 앱의 구성 정보가 포함된 파일로 모든 ios 앱이 가지고 있습니다.
	- 프로젝트 생성 시 Xcode가 자동으로 파일을 만들고 주요 속성값을 설정합니다.
	- 필수 키 값들
		- 번들 표시 이름  : 지원되는 모든 언어에 대해 현지화되어야함
		- 번들 식별자 :  앱 서명의 유효성 검사에 사용되는 역방향 DNS 값입니다. ex) com.akapuch.artistry
		- 번들 버전 : 번들 빌드 버전 번호로, 현지화할 수 없습니다.
		- 번들아이콘 파일 : 앱 아이콘에 사용되는 이미지 이름이 포함된 문자열 배열
		- Application requires iOS environment : ios에서만 가능한지 여부 판별하는 Bool값(Default : true)
		- UIRequiredDeviceCapabilities : 아이튠즈, 앱스토어에 앱의 필수기능을 알리는 키로, 사용자의 장치가 기능을 지원하지 못한다면 설치할 수 없도록 만듭니다.

	- 권장 설정 키 값들
		- 메인 nib 파일 이름 : 사용자 설정 nib 사용시 변경해야 하는 값으로, 이름에는 확장자(.nib)를 포함x
		- UIStatusBarStyle : 상태표시줄의 스타일을 설정하는 값. 기본은 `UIStatusBarStyleDefault`
		- UIStatusBarHidden : 상태 표시줄 숨길지 여부 설정(default : false)
		- UIInterfaceOrientation : UI의 초기 방향 설정 값
		- UIPrerenderedIcon : 아이콘의 광택 및 경사효과 포함 여부(default : false)
		- UIRequiresPersistentWifi : 앱의 Wifi 사용 여부를 알리는 값으로, 미 설정 시 30분 후에 전원절약을 위해 자동으로 Wifi 연결을 종료시킵니다.
		- UILaunchImageFile : 시작 이미지의 이름 키 값으로, 미설정시 Default 파일로 설정됩니다.

- 실행 파일(필수)
	-  애플리케이션의 기본 진입점과 애플리케이션 대상에 정적으로 연결된 모든 코드가 포함되어 있습니다.

- 리소스 파일(권장)
	- 리소스는 애플리케이션의 실행 파일 외부에 있는 데이터 파일로서 일반적으로 이미지, 아이콘, 사운드, nib 파일 , 문자열 파일, 구성 파일, 데이터 파일 등으로 구성됩니다. 대부분의 리소스 파일은 특정 언어 또는 지역에 대해 현지화되거나 모든 현지화에서 공유될 수 있습니다. 리소스 파일의 디렉토리 위치는 ios, macOS 별로 상이합니다.
	- 필수는 아니나 ios는 앱 아이콘 및 기본 화면에 대한 추가 이미지 리소스가 필요하기 때문에 사실상 권장사항입니다.

- 기타 지원 파일
	- ios는 사용자 정의 데이터 리소스를 포함할 수 있지만 사용자 정의 프레임워크 또는 플러그인은 생성 불가하기 때문에 포함되지 않습니다.
	- Mac 앱은 비공개 프레임워크 , 플러그인, 문서 템플릿 및 응용 프로그램에 필수적인 기타 사용자 지정 데이터 리소스가 포함되기도 합니다. 

## 참고문헌 및 읽을거리

### 참고문헌 
- [번들 프로그래밍 가이드](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1)

### 읽을거리
- [프레임워크 번들에 대하여..](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/FrameworkAnatomy.html#//apple_ref/doc/uid/20002253-BAJEJJAB)
- [로드가능한 번들 및 플러그인 아키텍쳐](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingCode/Concepts/AboutLoadableBundles.html#//apple_ref/doc/uid/20001268-BCIDBAEJ)