# DispatchQueue.main.sync를 사용하는 상황을 설명하시오.

## Deadlock(교착 상태)

<image src = "https://github.com/Swift-Master/SwiftMaster17_Team13_Study/assets/116094622/b2b4d893-ca11-4af6-9396-3dd1be890b66" width = 30%></image>


- 위 그림에서 차선을 대기열(queue), 차를 task라고 상상하고 생각해보자. 
- 양 쪽 모두 상대방 차선에 자리가 생기길 기다리며 정체되어 있는 상태이다.
- 코드로 알아봅시다.

```swift
let myQueue = DispatchQueue(label: "label") // 명시하지 않으면 default는 serial

myQueue.async { // task1
  print(1)
  myQueue.sync { // task2
	  print(2)
      // 외부 블록이 완료되기 전에 내부 블록은 시작되지 않습니다.
      // 외부 블록은 내부 블록이 완료되기를 기다립니다.
  }
  print(3)
} // task1의 종료시점
```
- 한번에 하나의 task씩 처리되는 serial 대기열에서의 교착상태 예시입니다.
- 흐름은 다음과 같습니다.
	- 가장 먼저, myQueue 대기열에 async로 task1을 제출합니다. 클로저의 첫 코드 1을 print합니다.
	- myQueue 대기열에 sync로 task2를 제출합니다. sync로 serial 대기열에 작업을 제출했기 때문에 대기열의 현재 실행중인 작업(task1)이 끝날때 까지 기다리게 됩니다.
	- 그러나 myQueue.async{}, 즉 task1은 클로저의 폐행까지 가야 종료될 것이고 그러기 위해서는 내부 task2가 작업을 마치고 print(3)까지 실행해야 합니다.
	- 결국 도로 그림 예시처럼 task1은 task2가 끝나기를, task2는 task1이 끝나기를 하염없이 기다리며 교착상태에 이르게 됩니다.

## DispatchQueue.main.sync의 용도?

- main 대기열은 앱이 실행되면서 자동으로 생성됩니다. serial하고 UI 관련 task들을 처리하는 main 쓰레드와 연결되어 있습니다.
- main 쓰레드는 UI 업데이트 task들을 main 런 루프로 전달하여 처리합니다. UI 업데이트는 60HZ 기준 1초에 60번 발생하고 끊어져선 안되기 때문에 main 런 루프는 무한히 반복되는 사이클입니다.
- 이러한 이유로 일반적인 경우 main 대기열에 sync로 task를 제출하게 된다면 task가 실행되는 동안 UI 업데이트를 지연시킬 수 있어 지양해야 합니다.
- 단, 백그라운드 대기열처럼 메인 쓰레드가 아닌 곳에서는 사용이 가능합니다.
- 다음은 PHPhotoLibrary의 메서드 `photoLibraryDidChange`의 구현 코드입니다.
```swift
func photoLibraryDidChange(_ changeInstance: PHChange) {
    guard let collectionView = self.collectionView else { return }
    // Change notifications may be made on a background queue.
    // Re-dispatch to the main queue to update the UI.
    DispatchQueue.main.sync {
        // Check for changes to the displayed album itself
        // (its existence and metadata, not its member assets).
        if let albumChanges = changeInstance.changeDetails(for: assetCollection) {
            // Fetch the new album and update the UI accordingly.
            assetCollection = albumChanges.objectAfterChanges! as! PHAssetCollection
            navigationController?.navigationItem.title = assetCollection.localizedTitle
        }
        // Check for changes to the list of assets (insertions, deletions, moves, or updates).
        if let changes = changeInstance.changeDetails(for: fetchResult) {
            // Keep the new fetch result for future use.
            fetchResult = changes.fetchResultAfterChanges
            if changes.hasIncrementalChanges {
                // If there are incremental diffs, animate them in the collection view.
                collectionView.performBatchUpdates({
                    // For indexes to make sense, updates must be in this order:
                    // delete, insert, reload, move.
                    if let removed = changes.removedIndexes where removed.count > 0 {
                        collectionView.deleteItems(at: removed.map { IndexPath(item: $0, section:0) })
                    }
                    if let inserted = changes.insertedIndexes where inserted.count > 0 {
                        collectionView.insertItems(at: inserted.map { IndexPath(item: $0, section:0) })
                    }
                    if let changed = changes.changedIndexes where changed.count > 0 {
                        collectionView.reloadItems(at: changed.map { IndexPath(item: $0, section:0) })
                    }
                    changes.enumerateMoves { fromIndex, toIndex in
                        collectionView.moveItem(at: IndexPath(item: fromIndex, section: 0),
                                                to: IndexPath(item: toIndex, section: 0))
                    }
                })
            } else {
                // Reload the collection view if incremental diffs aren't available.
                collectionView.reloadData()
            }
        }
    }
}
```

- 해당 메서드는 백그라운드 대기열에서 실행되는데, 내부의 `dispatchQueue.main.sync` 를 통해 UI관련 업데이트를 진행하게 됩니다.
- 이 메서드의 목적은 collectionView의 변화가 생기는 시점에 해당하는 cell에 변화를 반영시키는 것입니다.
- sync가 현재 대기열을 차단(제출된 작업이 끝날때까지 다른 작업을 실행할 수 없음)시키는 효과가 있지만 현재는 백그라운드 대기열에 있기 때문에 UI 업데이트에도 영향을 주지 않게 됩니다.
### async로 할 수 있지 않나요?
- 물론 async로도 문제없이 UI를 업데이트할 수 있습니다. 그러나 async는 작업 완료되기 전에 다음 작업을 실행하기 때문에 원하는 시점에 작업이 이뤄지지 않아 뜻하지 않은 오류가 발생할 수 있습니다.

## 참고문헌 
- [디스패치큐에 관해..](https://yagom.net/forums/topic/%EC%95%BC%EA%B3%B0%EB%8B%B7%EB%84%B7-%EC%A7%88%EB%AC%B8%EB%AA%A8%EC%9D%8C-6/)

- [사용사례- PHPhotoLibrary](https://developer.apple.com/documentation/photokit/phphotolibrarychangeobserver)

- [스택오버플로우](https://stackoverflow.com/questions/44324595/difference-between-dispatchqueue-main-async-and-dispatchqueue-main-sync)

- [미디움-sync의 사용용도](https://sarawanak.medium.com/demystifying-dispatchqueue-main-sync-b59b77e4f502)