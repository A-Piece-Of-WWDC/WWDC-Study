# Advances in UICollectionView
[Advances in diffable data sources](https://developer.apple.com/videos/play/wwdc2020/10045/) 섹션에서 본 예제 프로젝트인 **Emoji Explorer** 를 해당 섹션에서도 다시 한번 살펴본다.

<img width="359" alt="스크린샷 2022-12-14 오후 8 32 10" src="https://user-images.githubusercontent.com/53509789/208542142-3e7ab6c9-179f-488d-9ed3-89893a1d27c2.png">

- 섹션 1: 가로로 스크롤되는 이모지 그리드  
- 섹션 2: iOS14의 새로운 기능인 확장 및 축소가 가능한 Outline 스타일  
- 섹션 3: UITableView 처럼 보이는 리스트


## Collection View APIs
UICollectionView를 구성하는 API는 **Data**, **Layout**, **Present** 세 가지 범주로 나눌 수 있다. UICollectionView의 참신한 개념은 컨텐츠가 렌더링되는 data(what)와 layout(where)의 관심사를 분리하는 것이다.  

### iOS 6에서의 CollectionView

<img width="1680" alt="스크린샷 2022-12-22 오전 3 28 19" src="https://user-images.githubusercontent.com/53509789/208977545-a9faea73-d1ed-46d1-bdea-488c7f2b4253.png">

iOS 6에서 UICollectionView가 출시되었을 때,  
**Data**는 indexPath 기반 프로토콜인 UICollectionViewDataSource를 통해  
**Layout**은 UICollectionViewLayout을 상속받은 UICollectionViewFlowLayout를 통해  
**Present**은 UICollectionViewCell, UICollectionReusableView를 통해  
관리되었다.

### iOS 13에서의 CollectionView

<img width="1627" alt="스크린샷 2022-12-22 오전 3 27 27" src="https://user-images.githubusercontent.com/53509789/208977387-0f16f89e-3501-4a42-80a4-ee4b1faf3d14.png">

iOS 13에서는 DiffableDataSource와 Compositional Layout을 이용하여 Data와 Layout에 대한 새로운 구성 요소를 도입했다.

### iOS 14에서의 CollectionView

<img width="1680" alt="스크린샷 2022-12-22 오전 3 30 58" src="https://user-images.githubusercontent.com/53509789/208978030-fa552604-83d8-43a2-94cc-3b71742e680d.png">

iOS 14의 경우, 이 최신 API 기반에 새로운 기능들을 도입했다.

### 향상된 기능들 
(이전 주차 복습)
- **Diffable Data Source** (iOS 13 - 참고 정리본: [Advances in UI Data Sources](https://github.com/A-Piece-Of-WWDC/WWDC-Study/blob/main/3%EC%A3%BC%EC%B0%A8/Advances%20in%20UI%20Data%20Sources.md))
    - 스냅샷 데이터 유형 추가로 UI 상태 관리 단순화
    - 식별자를 통한 UI 상태 캡슐화
    - 추가 작업(batch update) 없이 자동으로 애니메이션 생성
- **Section Snapshot** (iOS 14 - 참고 정리본: [Advances in Diffable Data Sources](https://github.com/A-Piece-Of-WWDC/WWDC-Study/blob/main/5%EC%A3%BC%EC%B0%A8/Advances%20in%20Diffable%20Data%20Sources.md))
    - UICollectionView의 단일 섹션에 대한 데이터 캡슐화
    - Outline Style과 같은 UI를 지원하기 위해 필요한 계층 구조의 데이터 모델링 허용

<img width="500" alt="스크린샷 2022-12-22 오전 3 54 02" src="https://user-images.githubusercontent.com/53509789/208981842-d0de276c-8cc0-4b3b-8dbf-8480cfbb0b5d.png">

따라서 해당 예제의 경우 Diffable Datasource를 각각 단일 섹션의 컨텐츠를 나타내는 개별 섹션 스냅샷으로 구성할 수 있다.

- **Compositional Layout** (iOS 14)
    - iOS 13에서 도입된 Compositional Layout을 통해 작고 이해하기 쉬운 레이아웃 비트를 구성하여 풍부하고 복잡한 레이아웃을 구축할 수 있었다. (iOS 13 - 참고 정리본: [Advances in CollectionView Layout](https://github.com/A-Piece-Of-WWDC/WWDC-Study/blob/main/3%EC%A3%BC%EC%B0%A8/Advances-in-Collection-View-Layout.md))
    - iOS 14에서는 Lists라는 새로운 기능이 도입되었다. 이는 Compositional Layout 기반으로 구축되었기 때문에 섹션별로 다른 종류의 레이아웃과 쉽게 혼합할 수 있다. 또한  UITableView에서 기대하는 기능 (Swipe, Cell Layout 등)들을 누릴 수 있다.

    - Compositional Layout List 코드 스니펫
<img width="1641" alt="스크린샷 2022-12-22 오전 4 09 51" src="https://user-images.githubusercontent.com/53509789/208984453-080d5a36-fdda-46c4-aeed-c110fd972c62.png">
<img width="200" alt="스크린샷 2022-12-22 오전 4 12 50" src="https://user-images.githubusercontent.com/53509789/208984953-d5e37850-5d53-40b7-bcf5-66de59a0f0f2.png">
    - 그 외에도 UICollectionViewListCell, Header/Footer, iPadOS 앱에서 새로운 Sidebar 모양이 있다. (iOS 14 - 참고 정리본: [Lists in UICollectionView](#lists-in-uicollectionview))
    

## Modern Cells 

### Cell registrations
iOS 14에서는 Cell registrations 이라는 새로운 API로 셀을 구성할 수 있다.  
이는 ViewModel로 부터 셀을 설정하는 간단하고 리유저블한 방법이며 이는 셀 클래스나 nib을 등록하는 추가 과정을 생략할 수 있게 해준다. 대신 ViewModel로 부터 새 셀을 설정하기 위한 configuration 클로저를 사용하는 제네릭 registration type을 사용한다. 

해당 API를 살펴보자.

<img width="1680" alt="스크린샷 2022-12-22 오전 4 34 30" src="https://user-images.githubusercontent.com/53509789/208988423-390999e6-d857-4d99-ae21-a5e7f09cdb74.png">

- 셀 클래스와 ViewModel 클래스에 대해 제네릭 CellRegistration을 만든다.
- CellRegistration 될때마다 호출되는 클로저를 이용하여 ViewModel로부터 셀의 컨텐츠를 구성해준다.
- 새로운 API dequeueConfiguredReusableCell와 DiffableDataSource의 셀 provider의 셀 등록을 이용하면 된다.

### Cell Content Configuration

- 셀에 대한 표준화된 레이아웃을 제공한다. (UITableViewCell과 비슷)
- 모든 셀에 사용될 수 있으며 일반적인 UIView에도 사용이 가능하다.

1. `UIListContentConfiguration.cell()`
<img width="1676" alt="스크린샷 2022-12-22 오전 4 56 35" src="https://user-images.githubusercontent.com/53509789/208992245-f2f3b217-a8fe-434c-adc6-c46ee840c292.png">

2. `UIListContentConfiguration.valueCell()`
<img width="1671" alt="스크린샷 2022-12-22 오전 4 58 44" src="https://user-images.githubusercontent.com/53509789/208992572-8c6545ea-f34e-4d49-8373-76363835222b.png">


3. `UIListContentConfiguration.subtitleCell()`
<img width="1680" alt="스크린샷 2022-12-22 오전 4 59 44" src="https://user-images.githubusercontent.com/53509789/208992713-de99ea7b-d931-4472-8906-0d7287d22509.png">  

