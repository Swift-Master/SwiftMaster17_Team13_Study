# NSOperationQueue 와 GCD Queue 의 차이점을 설명하시오.

## NSOperationQueue란?

### 특징
- 높은 수준의 추상화가 적용되어 있는 Objective-C 클래스입니다.
- 큐에서의 작업단위 기준은 NSOperation 객체 1개입니다.
- GCD를 obejctive-c로 랩핑한 것이 결국 NSOperation이기 때문에 NSOperation을 사용한다면 내부에서 GCD Queue를 호출하여 사용하게 됩니다.
- 선입선출(FIFO)로 작업을 수행합니다.
- GCD의 QOS처럼 큐의 전체 작업들의 우선순위를 지정 가능합니다.
- 큐의 기본값은 concurrent queue입니다.
### 장점
- GCD에서는 불가능한 유용한 기능들(취소, 다시시작,중지)을 지원합니다.
- 작업들간의 Dependency를 설정하여 좀더 세밀하게 작업의 순서를 정할 수 있습니다.
- KVO(Key-Value Observing)을 통해 큐의 정보를 확인가능합니다.
	- maxConcurrentOperationCount : 최대 동시 작업 가능 수
	- 작업의 상태(일시중지 상태인지 확인)
	- 큐의 이름
### 단점
- 내부적으로 GCD를 호출하긴 하나 다른 오버헤드가 많이 발생하기 때문에 더 느립니다.
- NSOperation 객체를 생성하여 여러 설정들을 해줘야 하기 때문에 GCD에 비해 신경 쓸 것들이 많습니다.

## GCD Queue란?

### 특징
- C기반의 낮은 수준의 API로 직접 시스템과 상호작용합니다.
- 큐에서의 작업단위 기준은 코드 블록 하나입니다.
- 기본값은 serial queue입니다.
### 장점
- 추상화 단계가 낮고 low level api이기 때문에 속도가 빠르고 메모리에 부담이 적습니다.

### 단점
- 하나의 큐 전체 작업들의 우선순위를 정해줄 수 있으나 내부 작업들간의 상관관계까지 설정하는 것은 불가능합니다.
- 작업을 큐에서 실행시켰다면 일시정지, 다시시작, 취소 등이 불가능합니다.

## 요약
간단하게 표로 정리해 볼 수 있습니다.
|| NSOperationQueue	| GCD Queue	|
|---|----|---|
|작업단위|NSOperation()|코드블록|
|추상화수준|높음(Obejective-C)|낮음(C)|
|속도|오버헤드로 인해 느리다|시스템단에 직접 전달되어 빠르다|
|구현 난이도|어렵다|간단|
|전체 작업 우선순위|queuePriority 속성 설정|qos 설정|
|큐 내부 작업간 순서|Dependency로 설정가능|일괄 FIFO|
|작업의 취소,일시중지,다시시작|NSOperation()|코드블록|
|기본 Queue|concurrent|serial|
|KVO|가능|불가능|

작업간의 순서가 필요하고 진행되는 와중에도 세밀한 컨트롤이 필요하다면 NSOperationQueue, 메모리 효율성을 위해 background에서 작업을 하는 것이 목적이라면 GCD를 쓰는 것이 권장됩니다.

## 참고 문헌
- [GCD, NSOperationQueue 비교](https://medium.com/@wailord/a-high-level-primer-on-ios-concurrency-df3bca3a6993)
- [ios 멀티스레딩 및 GCD, NSOperationQueue 비교](https://abhimuralidharan.medium.com/understanding-threads-in-ios-5b8d7ab16f09)
- [NSOperationQueue 애플 공식문서](https://developer.apple.com/documentation/foundation/nsoperationqueue/)
- [스택오버플로우- NSOperation vs GCD](https://stackoverflow.com/questions/10373331/nsoperation-vs-grand-central-dispatch) 