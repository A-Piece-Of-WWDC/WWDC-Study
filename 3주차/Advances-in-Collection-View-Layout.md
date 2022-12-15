# Why?
- UICollectionView 는 iOS6때 도입되었음
- CollectionViewFlowLayout 은 매우 유용했고 지금도 유용함.
- 그런데 요즘은 앱은 기기도 매우 다양해지고, 화면 크기가 달라짐에 따라 뷰가 많이 복잡해짐
→ 그래서 커스텀 레이아웃의 필요성이 생겼지만, 커스텀 레이아웃을 구축하는 경우 추가적인 문제가 발생함
    1. 컬렉션뷰에서 관리할 수 있는 서플리멘터리 뷰나 데코레이션 뷰는 구현 까다로움
    2. 셀프 사이징 문제
- 그래서 새로운 레이아웃 클래스, `CompositionalLayout`가 등장

**장점**
1. 복잡한 레이아웃을 선언형 API 로 간단하게 구축할 수 있음
2. 하나의 컬렉션 뷰로 다양한 레이아웃을 구성할 수 있음
3. 빠른 속도

# CompositionalLayout
- `UICollectionVIewCompositionalLayout`는 iOS13부터 사용 가능
- 이 컴포지셔널 레이아웃을 사용하기 위해서는 size, item, group, section의 개념이 필요
- 왜냐면 밑에 코드를 보면 레이아웃을 만들기 위해선 섹션이 필요하고,, 섹션에는 그룹,, 그룹에는 아이템,, 아이템에는 사이즈가 필요하기 때문이다..!
<img src="https://i.ibb.co/7SF6YGw/2022-12-01-5-08-54.png">

- `Item`: 하나의 Item (Cell)
- `Group`: Item의 집합
- `Section`: 여러 Group의 집합
- 모든 싱글 컴포지셔널 레이아웃은 동일한 네가지 기본 부분으로 구성되고, 예제에서도 동일한 패턴으로 작동

<img src="https://i.ibb.co/kKpB3PY/2022-12-01-1-05-48.png">
<img src="https://i.ibb.co/84mMsYF/2022-12-01-1-05-59.png">

<br>
<br>

## 1️⃣ Size
<img src="https://i.ibb.co/ZVWXL2V/2022-12-01-5-02-34.png">
<img src="https://i.ibb.co/t863qRn/2022-12-01-4-17-58.png">

- 자신을 감싸고 있는 컨테이너에 비례한 값을 가지는 `fractional`값

<img src="https://i.ibb.co/b6zwYnm/2022-12-01-4-20-10.png">
<img src="https://i.ibb.co/nmD3zqS/2022-12-01-4-20-15.png">

<br>


- Size의 width나 height처럼 절대적인 값을 지니는 `absolute`값

<img src="https://i.ibb.co/fr6yP5v/2022-12-01-4-22-42.png">
    
<br>

- `estimated` 즉, flexible한 추정 값
run time에 크기가 변할 가능성이 있는 경우 사용! 시스템이 예상 크기를 기반으로 실제 크기를 계산함


<img src="https://i.ibb.co/t863qRn/2022-12-01-4-17-58.png">


Size를 구성하는 LayoutDimension를 알았으니, Size를 통해 Item의 크기를 결정할 수 있음.

<br>

<img src="https://i.ibb.co/hMQnbLW/2022-12-01-4-50-00.png">

<br>

## 2️⃣ Item
<img src="https://i.ibb.co/z2XnTKR/2022-12-01-4-57-12.png">
앞서 구성한 사이즈, 거기다가 콘텐츠 인셋을 넣어서 아이템도 완성하면 이제 이렇게 됨
<img src="https://i.ibb.co/c8Pr8R7/2022-12-01-5-02-36.png">

<br>

## 3️⃣ Group
<img src="https://i.ibb.co/xjmp7Kf/2022-12-01-5-01-20.png">
<img src="https://i.ibb.co/XZ6Y5Qm/2022-12-01-5-03-03.png">


<br>

## 4️⃣ Section
<img src="https://i.ibb.co/25WvxWN/2022-12-01-5-04-46.png">
<img src="https://i.ibb.co/WFmxkrG/2022-12-01-5-07-41.png">

# Demo
<img src="https://i.ibb.co/MMfJNqW/2022-12-01-8-33-19.png" width=30%><img src="https://i.ibb.co/Kry8Zv4/2022-12-01-8-35-05.png">

<br>

# 추가 고오급 기능
## UICollectionLayoutSupplementaryItem

- 뱃지 - SupplementaryItem에는 containerAnchor라는 전달인자가 있는데, 여기에 뱃지 위치를 넣으면 됨
<img src="https://i.ibb.co/fthjStz/2022-12-06-8-13-54.png">

- 헤더, 푸터 - pinToVisibleBounds는 SupplementaryView를 고정시켜주는 옵션
<img src="https://i.ibb.co/5Kd9YZw/2022-12-06-8-14-06.png">

<br>

## NSCollectionLayoutDecorationItem
<img src="https://i.ibb.co/RCqpB4N/2022-12-06-8-14-11.png">
<img src="https://i.ibb.co/TcJQsZ5/2022-12-06-8-14-17.png">



<br>

## 섹션 스크롤 옵션
<img src="https://i.ibb.co/bN7Q2ZZ/2022-12-01-5-24-13.png">

- **`.continuous` 연속적으로 scroll이 가능한 옵션**
- `.continuousGroupLeadingBoundary`
연속적으로 scroll이 가능하지만 각 Group의 LeadingBoundary에서 자연스럽게 멈추는 옵션
- `.paging` 각 컨텐츠가 기본 paging 처리되는 옵션
- `.groupPaging` 한 group을 기준으로 paging 처리되는 옵션
- `.groupPagingCentered` 한 group의 중심을 기준으로 paigng 처리되는 옵션
- `.none`