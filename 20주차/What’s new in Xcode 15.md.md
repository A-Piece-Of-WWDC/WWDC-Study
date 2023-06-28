# What’s new in Xcode 15

- Xcode 15 beta에서 업데이트 되는 기능들에 대한 세션

# Editing

### Code Completion

- 자동 코드 완성 기능이 좀더 스마트해졌다
    
    <img width="1904" alt="스크린샷 2023-06-14 오후 2 49 29" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/00112c4d-929a-43e3-9bb5-46c0f608ddb3">

    - 예를 들어, PlantSummaryRow라는 파일을 생성했을 때, 아직 해당 타입을 만들지 않았음에도 위 스크린샷처럼 유저가 만든 파일명을 통해 PlantSummaryRow라는 자동완성을 제공하고 있음
        
        <img width="1684" alt="스크린샷 2023-06-14 오후 3 25 52" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/fef3fe0a-85e4-4195-a4cf-7e63f04d0584">

    - 또, 다양한 파라미터를 포함시킬 수 있는 특정 메서드의 경우, 키보드의 → 버튼을 활용하여 원하는 파라미터만 유저가 선택할 수 있음
        
        <img width="1685" alt="스크린샷 2023-06-14 오후 3 31 39" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/15433ca7-9b82-43a9-8835-33990f534e8b">

        - frame 메서드의 경우, 위와 같이 자동완성에서 다양한 파라미터 옵션을 제공해주고 있음
    - 자동완성에서 Suggestion 기능도 좀 더 고도화됐음
        - 가장 최상단 Suggestion은 특정 View에서 Xcode에서 추천해주는 가장 빈번하게 사용되는 메서드를 추천해줌
            
            <img width="1641" alt="스크린샷 2023-06-14 오후 3 35 05" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/1045376a-1ec9-42d2-b979-da23ff8785ab">

        - padding은 View에서 가장 자주 쓰이는 modifier이므로 가장 상단에 위치해있는걸 확인할 수 있음
        - 만약 padding을 사용했다면, padding은 더 이상 suggestion에 뜨지 않고 2순위의 modifier가 추천됨.
        - 또, 만약 같이 묶여서 자주 사용되는 프로퍼티들이 있다면, 해당 요소도 고려하여 suggestion해줌
            - 예를 들어, CLLocation의 latitude를 썼다면, 다음으로는 자연스럽게 longitude를 필요로 하기 때문에 suggestion 상단에 longitude가 위치하게됨
                
                <img width="1601" alt="스크린샷 2023-06-14 오후 3 37 55" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/dffaddf3-43f1-4b93-9807-a1164303d6a2">

    
    - 이제 Asset을 사용할 때도 자동완성 기능이 제공됨
        
        <img width="1158" alt="스크린샷 2023-06-14 오후 3 40 20" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/0d2be1b8-5a05-47a2-811b-683780271b9d">

        <img width="789" alt="스크린샷 2023-06-14 오후 3 40 33" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/d3d001fb-2e7c-47b6-9de5-ea1b3bb5c4fb">

        - 예를 들어, MultipleClouds라는 이미지 네임이 있을 때, 해당 이미지 네임을 String name대신 직접 참조해서 가져올 수 있음.
            - 많은 휴먼 에러가 방지될듯
    

### String Catalog

- Localization기능의 고도화(String Catalog가 추가됨)
    - String catalog를 활용하여 우리는 이제 모든 localization을 한 곳에 모을 수 있게됨
        
        <img width="675" alt="스크린샷 2023-06-14 오후 3 47 07" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/a55329f4-eef6-47b0-92a2-31fc347970bf">

    - 기존의 strings 파일들을 string catalog로 migrating하는 기능도 제공하고 있음
        
        <img width="1715" alt="스크린샷 2023-06-14 오후 3 48 23" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/bc602d53-2e87-4587-b58a-427cfbece763">

    - String catalog는 위 스크린샷과 같은 형태. 앱이 지원하는 모든 나라의 언어를 커버하고 있는지 state를 통해 확인할 수 있음.
        
        <img width="381" alt="스크린샷 2023-06-14 오후 3 50 04" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/df4c0485-774e-4ab1-9d5e-e221e8849629">

    - 또한 좌상단의 퍼센트표시를 통해, 얼마나 커버되고 있는지도 확인 가능함
        - String catalogs에 대해 자세한 내용은 Discover String Catalogs 세션 참고
        

### Documentation Preview

- Assistant에 **Documentation Preview**가 추가됨
    
    <img width="1651" alt="스크린샷 2023-06-14 오후 4 24 14" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/a545ff9c-7bea-41d5-b49a-85cb847d6de7">

    <img width="1674" alt="스크린샷 2023-06-14 오후 4 24 39" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/3b2d14ca-2dcc-479c-b6a6-b3f13c4ebe8f">

    - 유저가 documentation을 작성할 때, 이제 preview를 통해 실시간으로 확인할 수 있음
    - 해당 내용 좀 더 자세히 볼려면 Create rich documentation with Swift-DocC 세션 참고
    

### Macros

- **Swift Macros**
    - Swift에 새롭게 추가된 feature인 Swift macros. 해당 기능을 Xcode 15에서 완벽하게 integration 해주고 있음
    - 매크로는 Swift Package의 일부로서 Swift Package를 통해 기능이 제공됨
        
        <img width="1898" alt="스크린샷 2023-06-14 오후 4 55 35" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/644b11e8-1794-4e35-a46d-525195410a27">

    - 예를 들어, 위와 같은 EnumHelper라는 패키지에 열거형의 각 case를 detect해주는 CaseDetectionMacro를 만들었을 때, 우리가 사용할 때는 아래와 같이 적용하면 됨.
        
        <img width="1878" alt="스크린샷 2023-06-14 오후 5 06 17" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/f5e3110a-043c-473a-bccc-ec7c40980ee6">

    - 위 스크린샷 처럼, 매크로를 적용하려는 열거형 선언부의 상단에 감싸주면됨
        - 원래 isOne이라는 프로퍼티는 존재하지 않는데, 매크로에 의해서 생성된 프로퍼티라는걸 알 수 있다
    - 만약 매크로를 사용한 부분에서 버그가 발생하거나 디버깅을 할 필요가 있을 때, 우리는 매크로를 확장시켜줄 수도 있음
        
        <img width="583" alt="스크린샷 2023-06-14 오후 5 09 02" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/e3ee38ad-8acf-445a-bd1b-0f0fb6624611">

        - Xcode 15에 새롭게 추가되는 Quick Action(Cmd + Shift + A)을 활용하여 매크로를 확장시켜줄 수 있다
            
            <img width="1623" alt="스크린샷 2023-06-14 오후 5 10 04" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/6514ff34-19e7-4801-b5f5-07b81bf745e0">

        - 확장시켜주면 위 스크린샷처럼, 매크로가 적용된 코드를 확인할 수 있고, 브레이크 포인트도 찍을 수 있다 !
    

### #Preview

- Preview 관련 API가 추가됨.
    
    <img width="1679" alt="스크린샷 2023-06-14 오후 5 55 24" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/7af519f9-7307-443d-8511-d9c3480de583">

    - #Preview를 활용하여 기존 PreviewProvider보다 좀 더 간편하고 직관적으로 사용할 수 있음
    - 다양한 프리뷰를 보고 싶을 경우 각각의 프리뷰를 구분할 수 있게끔, Preview에 이름도 붙여줄 수 있음
    - 무엇보다도, 이제 UIKit에서도 프리뷰를 사용할 수 있게됨
        
        <img width="1730" alt="스크린샷 2023-06-14 오후 5 57 57" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/ed777be9-5342-4767-bbb6-027345af23ce">

    - 위젯에서도 가능함
        
        <img width="1704" alt="스크린샷 2023-06-14 오후 5 59 10" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/7744648b-3f69-4ef8-b10a-4239be72bfe3">

        - 위젯의 모든 entry가 프리뷰로서 제공되며, 각각의 entry 클릭시 프리뷰에서 즉각적으로 애니메이팅 되면서 반응함
    - Xcode Preview와 관련한 자세한 내용은 Build programmatic UI with Xcode Previews 세션 참고

# Navigating

### Bookmark Navigator

- 우리의 프로젝트가 확장되고 거대해질수록, 우리는 디버깅을 하거나 어떤 소스 코드를 찾아야할 때, 각각의 파일들을 navigating 하는 과정이 복잡하고 힘들어짐(기억력의 한계)
- 이러한 점을 보완하기 위해 Xcode 15에서 bookmarks navigator 기능이 추가되었음
    - 북마크 에디터는 내비게이터 영역의 3번째에 위치해있음
        
        <img width="364" alt="스크린샷 2023-06-14 오후 6 07 34" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/82702ac3-cf1e-4e89-a93d-1233917a228b">

    - 북마크 설정은 내가 북마크를 설정하고 싶은 라인에 우클릭을 해서 설정해줄 수 있다
        
        <img width="799" alt="스크린샷 2023-06-14 오후 6 08 22" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/57935dfb-9703-419e-9a3f-262ca541dc81">

    - 북마크를 설정하면 navigator에 표시가 되고 해당 라인에 북마크 표시가 나오게 된다
        
        <img width="961" alt="스크린샷 2023-06-14 오후 6 13 32" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/42ed13ba-5ca4-477a-b49a-740109ec7339">

        <img width="365" alt="스크린샷 2023-06-14 오후 6 14 32" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/8befd9f5-c468-4a02-bf17-a7eb814c8a45">

        <img width="1912" alt="스크린샷 2023-06-14 오후 6 15 00" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/fb98e374-198e-4a0f-8255-49c234849311">

    - 북마크에 description도 붙여줄 수 있음.
        
        <img width="368" alt="스크린샷 2023-06-14 오후 6 17 45" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/ebea5cc1-bbda-4d02-96fb-2ad46532ab64">

    - 특정 북마크끼리 모아서 그룹화 하는 것도 가능하다
        
        <img width="186" alt="스크린샷 2023-06-14 오후 6 20 08" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/bfd480ff-3a9e-4b5d-9bd5-953b26c435f6">

    - 하나 더, 유용하고 놀라운 점은 위 스크린샷처럼 TODO 리스트 처럼 활용도 가능하다 !(이제 TODO 주석을 달 필요가 없다..!)
        
        <img width="1145" alt="스크린샷 2023-06-14 오후 6 21 18" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/c0fb8d37-4367-44db-99a2-878011efae03">

    - 또, 북마크 에디터에서는 쿼리 검색 기능도 제공함. 예를 들어, TODO를 검색해서 해당 TODO 리스트들을 북마크 에디터에서 관리가 가능하다 !
    

### Find Navigator

- find Navigator에도 새로운 기능이 추가됨.
- 특정 프로토콜을 채택하고 있는 클래스나 구조체, 열거형 등을 찾아줄 수 있다
    
    <img width="379" alt="스크린샷 2023-06-14 오후 6 25 03" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/6d7508b3-b450-4c16-a1c3-be72c6dbe520">

    - Conforming Types 를 활용하여, 해당 프로토콜을 사용하는 객체들을 찾아준 모습.
        
        <img width="193" alt="스크린샷 2023-06-14 오후 6 26 48" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/466a7f68-90ad-4f7a-9758-9aafcf07cff1">

    - 또한 이렇게 찾은 요소들을 북마크로 처리해줄 수도 있다 !
    - 만약 변화가 생긴다면, 리프레시 버튼 한 번 눌러주면 끝
        
        <img width="367" alt="스크린샷 2023-06-14 오후 6 27 30" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/c5058bbc-261a-49cd-a53a-87ce151d13dd">


# Sharing

### Source Control Editor

- 소스 컨트롤 에디터의 기능이 조금 더 고도화됨
    
    <img width="1919" alt="스크린샷 2023-06-14 오후 6 30 44" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/59a87c96-8222-4acd-ac96-b0a64c5e0ab7">

    - 이제 모든 소스 코드 수정 사항을 하나의 Scrolling View에서 다 확인할 수 있음
    - 소스 코드 에디터에서 바로 수정도 가능하다

# Testing

- Xcode의 Testing 속도가 무려 45%나 빨라졌다고 한다.
- Xcode나 Xcode Cloud를 활용하여 테스트를 했을 때, Test Report가 발행되게 된다
    
    <img width="1918" alt="스크린샷 2023-06-14 오후 6 50 04" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/64b96cc6-52e0-40f2-8ee3-0ca2ca557ef3">

- 위 스크린샷의 형태처럼 전체적인 테스트 결과에 대한 요약을 확인할 수 있음
- 특정 테스트의 실패에 대한 디버깅을 하고 싶을 경우, Test Detail도 확인해줄 수 있음
    
    <img width="880" alt="스크린샷 2023-06-14 오후 6 52 48" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/45b1a7d5-d5c1-4c29-8e2c-5e8a7ba5eef9">

- 위 스크린샷 처럼, 구체적인 테스트 과정이 타임라인별로 상세하게 표현되어 있고 각각의 테스트 장면이 포함되어 있다(!!)
- 이제 UI Test failure에 대한 디버깅이 꽤나 재밌어질듯
- Testing과 관련된 자세한 내용은 Fix failures faster with Xcode test reports 세션 참고

# Debugging

### OSLog

- 디버깅을 위한 프레임웍인 OSLog와 Xcode 15와의 integration이 더욱 긴밀하게 이뤄짐
    
    <img width="1623" alt="스크린샷 2023-06-14 오후 7 03 35" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/9cf991f4-ef4e-4d4d-b8f3-5011f3d7bdd3">

- 위와 같은 형태로 Logger객체를 생성하고 각 메서드를 활용하여 사용하는 형태
- Xcode 15의 콘솔은 OSLog를 fully Support 하고 있음. 따라서 콘솔을 통해 우리는 OSLog를 확인하고 추가적인 기능들을 사용할 수 있음
    
    <img width="1871" alt="스크린샷 2023-06-14 오후 7 08 09" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/b028150d-7096-46ad-901d-f6f06992a15c">

- OSLog를 활용하여 로깅을 하게되면 위와 같이 깔끔한 콘솔을 확인할 수 있다.
- 백그라운드 컬러가 지정된 로그는 치명적인 이슈일 경우 백그라운드 컬러가 지정됨
    - 이를 통해 우리는 막대한 양의 크리티컬한 메세지 로그를 보는 것보다 좀 더 간편하고 쉽게 문제를 식별할 수 있게됨
- 메타데이터 옵션도 켜줄 수 있다 !
    
    <img width="1162" alt="스크린샷 2023-06-14 오후 7 10 13" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/5ffc0bd9-44e9-4f45-8a5a-c4f8626909ae">

- 만약 특정 로그의 소스 코드로 이동하고 싶다면 콘솔에서 다이렉트로 이동도 가능함

<img width="927" alt="스크린샷 2023-06-14 오후 7 11 14" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/062d3b9b-7cb1-4902-ad84-9552e8a8d944">
<img width="1396" alt="스크린샷 2023-06-14 오후 7 15 07" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/a17a389b-6817-4e16-a005-860c8463d0a0">

- 좀 더 자세한 내용은 Debug with structured logging 세션 참고
