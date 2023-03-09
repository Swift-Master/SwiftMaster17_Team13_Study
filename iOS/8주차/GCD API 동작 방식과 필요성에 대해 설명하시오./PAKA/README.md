# GCD API 동작 방식과 필요성에 대해 설명하시오.

## GCD란?
- Dispatch, GCD(Grand Central Dispatch)라고도 부르기도 하는 이것은 apple환경(mac, tv, watch, ios)의 멀티코어 환경에서 실행되는 동시성 코드의 지원에 대해 시스템적/포괄적으로 향상시키는 언어의 기능, 런타임 라이브러리, 시스템적 향상을 모두 포함합니다.
- BSD subsystem, Core Foundation, Cocoa API에서 GCD를 사용하여 반응속도를 향상시켜 앱을 더 빠르고, 효율적으로 만듭니다.

## GCD 동작과정
1. 사용자가 DispatchQueue.`main()/global()`.`sync/async`{작업1()} 코드 작성 : 대기열의 종류, 작업의 우선순위, 이후 동작 등을 지정할 수 있습니다.
2. 런타임에 코드블럭 내부의 작업1()이 디스패치 대기열에 추가(작업 예약) : main()은 main 쓰레드에 존재하는 main 대기열로, global()은 concurrent 대기열에 작업이 추가됩니다.
3. GCD가 현재 대기열과 연결된 `thread pool`에서 사용가능한 쓰레드 탐색 : main 대기열은 main thread만 존재하지만, global 대기열은 여러 작업의 병렬 수행이 가능하기 때문에 가용 쓰레드 수에 따라 작업을 나눠서 할당할 수도 있습니다.
4. 탐색한 쓰레드로 작업 할당 및 동작유형에 따라 대기/ 다음 작업 수행 : sync()로 작업을 실행했다면 처음 코드를 실행했던 쓰레드는 디스패치시킨 작업1()이 끝날 때까지 기다려야 합니다.


### 유의할 점

1.  main 쓰레드에서 `sync{}`는 사용할 수 없습니다.

```
DispatchQueue.global().sync{
print("Ice Age") // 실행되지 않고 앱이 종료됩니다.
}
```
  - 위의 코드는 다음과 같이 동작합니다.
    -  작업을 메인이 아닌 쓰레드에서 실행시키고 그 작업이 끝날 떄까지 기다릴게.
  - 메인 쓰레드는 ios의 ui를 그리는 작업을 담당하고 있어 절대 작업을 멈출 수 없습니다.
  - 따라서 sync를 통해 메인 쓰레드가 대기하게 되면 런타임 에러가 발생합니다.

2. 현재 대기열과 같은 대기열로 `sync{}`로 작업을 디스패치하면 안됩니다.

``` 
DispatchQueue.global().async{
  task1() // global 대기열 내부에서 작업 실행
  DisPatchQueue.global().sync{  // 코드블록 내부의 작업(task2)이 끝날때까지는 현재 쓰레드 대기
       task2()  // 현재 쓰레드는 대기중이기 때문에 task2()는 절대 실행되지 않는다. `데드락 상태`
   }
}
```

3. closure의 캡처 현상 주의
 - 기본적으로 closure를 사용하여 여러 쓰레드를 넘나 들다보면 `self`가 자주 바뀌게된다.
 - @escaping나 전역변수에 값 할당 등으로 reference counting이 증가하여 의도치 않은 retain cycle이 발생할 수도 있으니 주의하자.
 - [GCD는 weak self가 필수일까](https://zeushin.github.io/2017/09/26/should-we-use-weak-self-in-gcd/), [weak self 알고쓰자](https://ios-development.tistory.com/926) 참고


 ## GCD의 필요성
### 메인 쓰레드는 너무 바쁘다
- 디스패치하지 않은 모든 작업은 기본적으로 main queue에 할당되고 있습니다.
- 또 60hz기준, 1초마다 화면을 60번씩 그리는 ui drawing 사이클도 수행하고 있기 때문에 매우매우매우 바쁩니다.
- 다른 대기열로 보냄으로써, 메인 쓰레드의 과부하를 막을 수 있습니다.

### 멀티 쓰레딩을 통한 성능 최적화
- main queue는 단 1개의 main thread와 연결되어 있으며 모든 작업을 순차적으로 처리합니다.(serial)
- GCD를 통해 전역 대기열(global Queue)로 작업을 보내면, 쓰레드 풀의 사용가능한 여러 쓰레드들로 작업들을 할당하고, 동시에 수행합니다. (concurrent)
- 데드락, 경쟁상태 등 동시성 이슈들이 발생할 수 있으나 단점에 비해 얻을 수 있는 성능적 이점이 크다.

## 참고 문헌
- [dispatch 공식문서](https://developer.apple.com/documentation/dispatch)
- [차근차근 시작하는 GCD](https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-grand-dispatch-queue-1-397db16d0305)
- [ios에서의 멀티쓰레딩](https://medium.com/@prasanna.aithal/multi-threading-in-ios-using-swift-82f3601f171c)
- [iOS-Swift에서 DispatchQueue 소개](https://medium.com/towardsdev/ios-introducing-dispatchqueue-in-swift-e9c6fbf8be1d)
- [GCD vs async/await](https://medium.com/towardsdev/swift-concurrency-deep-dive-1-gcd-vs-async-await-280ac5df7c76)
- [swift concurrent 심층 분석](https://medium.com/@manjunath.anawal3/gcd-in-swift-f1ae114d45fa)
