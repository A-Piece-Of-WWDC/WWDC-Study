# Advances in UI Data Sources

## Current state-of-the-art
오늘날 UICollectionView에서 UI DataSource와 상호 작용 하는법  

섹션 수, 각 섹션의 항목 수, 콘텐츠 rendering 시 셀 정보 요청
<img width="1427" alt="스크린샷 2022-12-01 오후 4 54 59" src="https://user-images.githubusercontent.com/53509789/204996913-92af207b-0245-4e29-a2c4-08083f9a6d3f.png">

UI 업데이트  
UI가 Controller의 numberOfItemsInSection을 통해 렌더링할 데이터를 받는다.

<img width="472" alt="스크린샷 2022-12-01 오후 5 42 57" src="https://user-images.githubusercontent.com/53509789/205006532-c58457d7-34c6-40cf-9984-a6bcc30deee3.png">

이 컨트롤러가 웹 서비스의 응답을 받는 등의 데이터 변경 사항이 생길때마다 UI에게 변경을 알리고 UI는 받은 변경 사항을 업데이트한다.

<img width="472" alt="스크린샷 2022-12-01 오후 5 54 11" src="https://user-images.githubusercontent.com/53509789/205008872-c736048e-4311-4f2c-8ddd-9f2fededcf3d.png">

하지만 이렇게 데이터 변경으로 인한 UI 업데이트 시, 다음과 같이 에러가 발생하는 경우가 생긴다.

<img width="503" alt="스크린샷 2022-12-01 오후 5 57 36" src="https://user-images.githubusercontent.com/53509789/205009628-633a5657-4526-49b7-a97a-7d11bcb32d5f.png">

우리는 `reloadData`를 호출하여 이 에러를 해결한다. 그러나 이 방식은 애니메이션을 발생시키지 않고 이것은 사용자의 경험을 저하시킨다.  

이러한 에러는 Data Source의 역할인 Data Controller가 시간이 지남에 따라 변하는 자신만의 truth를 가지고 있고, UI 또한 자신만의 truth를 가지고 있기 때문에 이 두 truth끼리 맞지 않아 나는 에러이다. (즉, centralize된 truth가 없기 때문)

<img width="357" alt="스크린샷 2022-12-01 오후 6 04 37" src="https://user-images.githubusercontent.com/53509789/205011256-f20d4f5a-d365-4605-bfd3-52d5d117e5cd.png">

## A new approach - Diffable Data Source
따라서 애플은 새로운 접근 방식 `Diffable Data Source` 를 도입한다.

### Diffable Data Source
<img width="472" alt="스크린샷 2022-12-01 오후 6 25 44" src="https://user-images.githubusercontent.com/53509789/205015840-2943933d-de6a-44e2-bb43-eebd67b1b670.png">

* `Diffable Data Source` 에는 `performBatchUpdates()` 메소드가 없으며 `apply()` 라는 단일 메소드가 존재한다.
    * [`performBatchUpdates()`](https://developer.apple.com/documentation/uikit/uicollectionview/1618045-performbatchupdates)  
    `CollectionView` 의 `insert, move, delete` 연산에 이 함수를 사용하여 각 연산의 변경과 동시에 animate를 줄 수 있다. 이 함수는 크래시를 유발하고 귀찮으며 복잡하다.  

   * `apply()`  
      쉽게 자동으로 diffing

### Snapshots
또한 Snapshot이라는 개념도 도입된다.

<img width="472" alt="스크린샷 2022-12-01 오후 6 37 56" src="https://user-images.githubusercontent.com/53509789/205018391-1171469a-c40d-464c-a165-0c62c94cc753.png">

이것은 현재 UI 상태에 대한 truth이다.  
이것은 `IndexPath` 대신 `section`과 `item`에 대한 `Unique identifiers`로 업데이트를 한다.

예시를 보자.

<img width="400" alt="스크린샷 2022-12-01 오후 6 41 20" src="https://user-images.githubusercontent.com/53509789/205019170-50816bdf-765e-402f-8824-9a6365390ed0.png"> <img width="199" alt="스크린샷 2022-12-01 오후 6 59 02" src="https://user-images.githubusercontent.com/53509789/205023545-04bfa190-dd03-4a68-ad6c-bee7a40c5c86.png">

BAR, FOO, BIF라는 `identifiers` 가 있고 `controller`가 변경되었다면, 새로운 SnapShot이 만들어지고 현재 상태에서 apply() 하여 새 스냅샷으로 업데이트 할 수 있다. 

우리는 모든 플랫폼에 걸쳐 4개의 클래스를 가지고 있다.
* iOS, tvOS
    * UICollectionViewDiffableDataSource
    * UITableViewDiffableDataSource
* MacOS
    * NSCollectionViewDiffableDataSource
* All Platform
    * NSDiffableDataSourceSnapshot

## Demos
우리는 `Diffable Data Source` 를 이용하여 데이터 변경이나 새로운 데이터를 넣고 싶을 때마다 스냅샷을 생성하고 그것을 (원하는 데이터로) 채우고 `apply` 하여 변경 사항을 자동으로 UI에 적용 시킬 수 있다. 그리고 이것은 매우 멋진 애니매이션과 함께 발생한다.

기본적으로 세단계가 존재한다.
1. 스냅샷을 만들고 
2. 필요에 따라 구성하고 
3. 적용한다.

<img width="596" alt="스크린샷 2022-12-01 오후 7 56 30" src="https://user-images.githubusercontent.com/53509789/205035397-b622b8b8-b117-4bad-90ca-3cd97df5de37.png">

1. NSDiffableDataSourceSnapshot을 생성한다.
2. Snapshot은 초기에 empty 이기 때문에 원하는 섹션과 항목으로 채우는 것은 우리에게 달려 있다. 현재 데모에서 우리는 보여줄 section이 하나이므로 item을 단일 section에 넣어준다. (이 업데이트에서 표시하려는 item의 idenfifier를 추가한다.)
3. datasource에게 이 snapshot을 apply()하는 메서드를 호출한다.

### Section, Item
* Hashable을 준수하는 타입이여야한다. 
    * Enum은 모든 케이스가 Hashable을 만족하면 Hashable! 
<img width="596" alt="스크린샷 2022-12-01 오후 7 57 42" src="https://user-images.githubusercontent.com/53509789/205035638-35664788-3f12-4f54-b416-7efc3f3331aa.png"><img width="534" alt="스크린샷 2022-12-01 오후 8 02 38" src="https://user-images.githubusercontent.com/53509789/205036579-bfb1c3bf-573f-42f1-a299-fc6a2b56964a.png">
  
마지막으로 cell을 만들고 데이터를 넣어준다.

<img width="596" alt="스크린샷 2022-12-01 오후 7 58 15" src="https://user-images.githubusercontent.com/53509789/205035758-f931d075-5ef4-4bb0-b754-64a226a5d493.png">

 
## Considerations
* **오직 `apply` 만을 통해 업데이트**

    <img width="521" alt="스크린샷 2022-12-01 오후 8 06 33" src="https://user-images.githubusercontent.com/53509789/205037297-bc5bc480-7702-412f-86e6-1608d3368c6f.png">

* **스냅샷을 만드는 방법**

    <img width="521" alt="스크린샷 2022-12-01 오후 8 08 22" src="https://user-images.githubusercontent.com/53509789/205037668-ff445008-87ca-44b0-853d-369692dff8e6.png">
    
    * 현재 상태의 스냅샷 사본 - 아주 작은 것 하나를 수정해야 하는 경우 유용

* **`IndexPath` 가 아닌  `identifier`**
    
    <img width="521" alt="스크린샷 2022-12-01 오후 8 52 58" src="https://user-images.githubusercontent.com/53509789/205046165-729503ce-61a2-4d2f-b41b-6c39ad8a241e.png">

* **성능**

    <img width="521" alt="스크린샷 2022-12-01 오후 8 54 02" src="https://user-images.githubusercontent.com/53509789/205046851-dfe249db-d897-4082-94a2-9bbf1a1e7f13.png">
    
    * 가능한 한 빠르게 만들었다.
    * 하지만 diffing은 linear한 동작이기 때문에 개발 단계에서 성능을 측정해보는 것이 매우 중요하다.
    * background 큐에서의 apply() 호출 또한 안전하게 작동할 수 있도록 고안했다.
    * main, background 큐 중 하나를 정해서 배타적으로 호출해야한다.
