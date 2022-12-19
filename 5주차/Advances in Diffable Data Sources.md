
### 예시 프로젝트 Emoji Explorer

<img width="359" alt="스크린샷 2022-12-14 오후 8 32 10" src="https://user-images.githubusercontent.com/53509789/208542142-3e7ab6c9-179f-488d-9ed3-89893a1d27c2.png">

- 섹션 1: 가로로 스크롤되는 이모지 그리드
- 섹션 2: iOS14의 새로운 기능인 확장 및 축소가 가능한 Outline 스타일
- 섹션 3: UITableView 처럼 보이는 리스트

### Diffable Data Source

<img width="674" alt="스크린샷 2022-12-14 오후 8 37 31" src="https://user-images.githubusercontent.com/53509789/208542165-bee015fc-35d3-4734-8a69-dac0d18bbe8d.png">

iOS 13에서 도입된 Diffable Data Source는 새로운 Snapshot 데이터 유형을 추가하여 UI 상태 관리를 크게 단순화했으며 이는 고유 Section 및 Item 식별자를 이용해 전체 UI 상태를 캡슐화한다.

이를 통해 개발자의 추가 작업 없이 차이를 계산하고 자동으로 애니메이션을 적용했다. 

## **iOS 14 에서 추가되는 두가지 기능**

iOS14에서는 Diffable Data Source를 기반으로 **Section Snapshot**과 **Recording Support** 기능이 추가 되었다.

### **Section Snapshot**

<img width="1577" alt="스크린샷 2022-12-14 오후 8 43 59" src="https://user-images.githubusercontent.com/53509789/208542171-7ac49b3c-7fc4-4b20-a853-bba6c23e7f6d.png">

Section Snapshot은 이름에서도 알 수 있듯이 단일 섹션에 대한 (Section 마다) 데이터를 캡슐화할 수 있다. 이러한 개선 이유는 다음과 같다.

1. 데이터 소스를 섹션 크기의 덩어리로 구성할 수 있다.
2. Outline Style과 같은 UI를 지원하기 위해 필요한 계층 구조의 데이터를 모델링 할 수 있다.

예시 프로젝트인 Emoji Explorer로 돌아가서, Section Snapshot을 사용하여 구축하는 방법에 대해 살펴보자. 3개의 Section Snapshot을 통해 Diffable Data Source를 구성할 수 있다.

**추가된 Section Snapshots 관련 API**

<img width="1608" alt="스크린샷 2022-12-14 오후 8 49 14" src="https://user-images.githubusercontent.com/53509789/208542177-ba81b7b7-e793-406e-a454-3329a72a4d18.png">

- iOS14의 Section Snapshot은 Item이 제네릭 타입이다.
- Section Snapshot에 컨텐츠를 추가하기 위해 append API를 사용한다.
    - 옵셔널 파라미터 parent가 제공된 경우, 계층 데이터를 모델링 하는 데 필요한 Section Snapshot에서 부모-자식 관계를 만들 수 있다.

또한 새로운 Section Snapshot 타입의 편의를 위해 UICollectionView DiffableDataSource에 두 개의 새로운 API apply, snapshot이 추가됐다.

<img width="1680" alt="스크린샷 2022-12-14 오후 8 52 50" src="https://user-images.githubusercontent.com/53509789/208542181-e8b43baa-5f20-42c2-9ef0-2b5a684c99cd.png">


- `apply`: Section Snapshot과 Section Identifier를 가져오는 메소드
- `snapshot`: 특정 세션의 컨텐츠를 나타내는 Section Snapshot을 검색할 수 있는 메소드

**Snapshot과 Section Snapshot을 함께 사용하여 Collection View의 컨텐츠를 구성하는 방법**

<img width="1512" alt="스크린샷 2022-12-14 오후 3 42 07" src="https://user-images.githubusercontent.com/53509789/208542184-9519932e-daaa-4e51-b551-2d5fc2bb4bad.png">

- Diffable Data Source에 Snapshot을 적용하여 원하는 순서대로 섹션을 추가한다.
    - 여기서는 recent, top, suggested 순서
- 섹션 순서를 정한 후 Section Snapshot을 각 섹션에 직접 적용하여 각 섹션에 대한 Item을 채운다.

**Section Snapshot API를 사용하여 Outline Style UI 구성**

<img width="1612" alt="스크린샷 2022-12-14 오후 8 57 16" src="https://user-images.githubusercontent.com/53509789/208542186-46fe2877-efd6-4aba-b232-f4a3d9ea8412.png">

- `append` 를 사용하여 Smiley, Nature, Food 등을 섹션에 추가
    - 옵셔널 parent 파라미터를 비워 루트 항목으로 추가
- 부모-자식 관계를 구성하기 위해 일부 자식 Item을 부모 Item에 추가
    - 상위 Item = Food

**Child snapshots**

<img width="1646" alt="스크린샷 2022-12-14 오후 8 58 19" src="https://user-images.githubusercontent.com/53509789/208542189-ca6baac9-8366-4d4c-8af3-09025a049399.png">

이제, 위와 같은 계층적 데이터의 일부에 대해서만 추론하거나 선택적으로 결과 Snapshot에 선택적으로 부모를 포함하여 특정 부모와 관련된 모든 자식 항목을 가져올 수 있다.

**Expanding and collapsing items**

계층 구조로 구성된 Section에서 사용할 수 있는 메소드들

<img width="1680" alt="스크린샷 2022-12-14 오후 8 59 00" src="https://user-images.githubusercontent.com/53509789/208542190-9d205d53-ce1d-45c4-b435-484277155a67.png">

- 확장 상태는 Section Snapshot 상태의 일부로 관리된다.
- 표시할 Snapshot을 만들 때, `expand`와 `collapse`라는 메소드를 통해 해당 Item의 부모 확장 상태를 설정하여 자식 컨텐츠가 처음에 표시되는지 여부를 쉽게 결정할 수 있다.
- 특정 항목이 확장되었는지 축소되었는지 `isExpanded` 메소드로 확인 할 수 있다.
- Section Snapshot의 확장 상태를 변경하면 Diffable Data Source에 실제로 `apply` 할 때까지 적용되지 않는다.
- 

<img width="762" alt="Untitled" src="https://user-images.githubusercontent.com/53509789/208542194-a748fc2f-39f1-46fd-a6be-be5d3414f02e.png">

사용자가 Outline Style UI와 상호 작용하면 프레임워크는 자동으로 Section Snapshot을 새로운 확장 상태로 업데이트하고 해당 Section Snapshot을 데이터 소스에 적용한다.

이러한 사용자 상호 작용으로 인한 확장 상태 변경에 대한 알림을 받고 이를 제어하기 위한새로운 API가 있다. 

- **Section snapshot handlers**

<img width="1502" alt="스크린샷 2022-12-14 오후 4 05 02" src="https://user-images.githubusercontent.com/53509789/208542199-5c7ef007-0a5c-4419-92bf-070422a329a2.png">

- “특정 부모가 절대로 축소되지 않아야 한다”는 디자인 요구사항이 있을 때, 이를 처리하기 위해 특정 부모에 대해 false를 반환하여 Outline UI의 축소를 방지하는 `shouldCollapseItem` 을 제공한다.
- `snapshotForExpandingParent` 를 사용하여 비용이 많이 드는 컨텐츠의 지연 로드를 지원한다. 이는 초기 Section Snapshot에 로드되는 컨텐츠의 양을 최소화하는 데 유용하다. 따라서 현재 자식 Snapshot의 상태에 따라 해당 컨텐츠를 로드할 수 있다.

### **Reordering Support**

<img width="931" alt="스크린샷 2022-12-14 오후 4 19 49" src="https://user-images.githubusercontent.com/53509789/208542209-bb431d98-c26d-49de-909b-f1dfce32afb9.png">

Diffable Data Source가 제공하는 고급 기능 중 하나는 컬렉션 보기의 데이터를 고유한 항목 identifier로 모델링 하는 기능이다. 이러한 identifier를 이용하여 프레임워크가 사용자 상호 작용을 기반으로 프로그램 대신 재정렬 변경사항을 자동으로 커밋할 수 있다.

하지만 이로는 충분하지 않고 응용 프로그램은 사용자가 재정렬 상호작용을 시작했음을 알려 프로그램의 시각적 순서를 백업 저장소에 유지해야한다. 이를 지원하기 위해 Diffable Data Source에는 이제 reorderingHandlers라는 새로운 속성이 있다.

<img width="1511" alt="스크린샷 2022-12-14 오후 4 25 37" src="https://user-images.githubusercontent.com/53509789/208542211-f5c45bf0-69d4-4f12-8dbf-e0f73c92e2e1.png">

- `canReorderItem` : 사용자가 재정렬 상호 작용을 시작하려고 시도할 때 호출되고 특정 항목을 재정렬할 수 있는지 여부를 결정
- `willReorderItem` : 해당 항목을 재정렬하기 위해 diffable 데이터 소스를 준비
- `didReorder` : 사용자가 재정렬 상호 작용을 완료하면 호출되고 새 순서 상태를 프로그램의 백업 데이터에 커밋할 수 있게 트랜잭션(NSDiffableDataSourceTransaction)을 처리

이 트랜잭션은 Diffable Data Source에 대해 수행되는 업데이트에 대해 추론하는 데 필요한 모든 정보를 제공한다.

<img width="1485" alt="스크린샷 2022-12-14 오후 4 34 06" src="https://user-images.githubusercontent.com/53509789/208542213-ed493b08-87f5-467f-8116-a83df98fe847.png">

이 유형은 4가지 기본 정보를 제공한다.

- `initialSnapshot`: 업데이트가 적용되기 전 Diffable Data Source의 상태
- `finalSnapshot` : 업데이트가 적용된 후 Diffable Data Source의 상태
    - 이 snapshot에서 Item 식별자를 직접 사용하여 앱의 백업 데이터에 커밋해야 하는 새로운 순서를 결정할 수 있다.
- `difference` : Swift 표준 라이브러리 Collection Difference가 지원된다.
- `sectionTransactions` : 재정렬 업데이트와 관련된 각 섹션에 대해 하나의 트랜잭션이 제공
    - **`NSDiffableDataSourceSectionTransaction`**
        - 이 sectionTransaction이 적용된 Section Identifier를 검사 할 수 있음
        - 이 섹션의 업데이트와 관련된 CollectionDifference와 함께 초기 및 최종 Section Snapshot, difference를 얻을 수 있다.

이제 이러한 트랜잭션과 함께 제공되는 Collection Difference를 사용하여 새로운 백업 데이터를 생성하고 업데이트하는 예시를 살펴보자.

<img width="1477" alt="스크린샷 2022-12-14 오후 4 45 00" src="https://user-images.githubusercontent.com/53509789/208542217-6718f35f-6e18-4cd4-a2e3-9413e17e8a20.png">

- `backingStore` : 단일 섹션에 대한 백업 데이터 Item의 배열
- 트랜잭션과 함께 제공된 Swift 표준 라이브러리 Collection Difference를 사용하여 새로운 데이터(`updatedBackingStore`)를 만들고 이를 백업 데이터에 직접 업데이트 할 수 있다.