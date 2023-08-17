# ****What’s new in UIKit (2023)****

- 새롭게 추가된 UIKit feature들에 대한 세션

# Key Features

## Xcode previews

- UIKit 환경에서 Preview를 direct로 사용할 수 있게 됨
- Preview 매크로를 사용하여 프리뷰를 생성하는 방식
    
    <img width="1798" alt="스크린샷 2023-07-12 오전 9 06 48" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/a80edca0-c9f6-4548-b97b-3d10d45f1dbb">

- UIViewController 뿐만 아니라, UIView에서도 사용 가능하다
    
    <img width="1823" alt="스크린샷 2023-07-12 오전 9 07 47" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/531686a2-c9c7-40ef-96fc-2f2721cb7471">


## View controller lifecycle updates

- `viewIsAppearing` 이라는 새로운 뷰컨트롤러 라이프사이클 메서드가 추가되었음
- `viewWillAppear`와 `viewDidAppear` 사이에서 호출되게 됨.
- `viewIsAppearing`의 중요한 포인트 중의 하나는 뷰가 hierarchy상에 모두 add 되고, 올바른 geometry값을 가지게 되고 난 뒤에 호출된다는 점.
- iOS 13 이상에서 지원되는 메서드.

<img width="1364" alt="스크린샷 2023-07-12 오전 9 16 52" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/5b6f62c9-ad6a-4359-b2f9-66e049d67d71">

- `viewIsAppearing`이 호출되는 시점은 은 뷰가 나타나는 시점에서 뷰를 업데이트 해주기 위한 좋은 시점이 될 것임
    - `viewWillAppear`는 뷰가 hierarchy에 추가되기 전이라 변화를 주기에는 너무 이름. `viewDidAppear`는 이미 뷰 전환과 관련된 모든 animation이 끝난 뒤이므로 뷰의 변화를 주기에는 너무 늦음.
- 그렇다면 `viewWillLayoutSubviews`나 `viewDidLayoutSubviews`를 활용하면 되지 않나? 라는 의문이 생길 수도 있는데, 해당 메서드들은 뷰 전환 중에 여러 번 불릴 수 있는 메서드들이므로 적절한 사용처가 될 수 없음.

## Trait system enhancements

<img width="855" alt="스크린샷 2023-07-12 오전 9 39 49" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/67210888-c8d5-4432-937f-01ac773047bc">

- Trait system에도 변화가 생겼음
- 이제 우리가 별도의 custom trait을 만들어서 정의해줄 수 있음.
- 새로운 trait override API가 추가되었음(그게 뭔지는 이 세션에서는 얘기 안해줌..)
    - 해당 메서드는 뷰나 뷰컨트롤러에서 trait value를 쉽게 수정할 수 있게 해주는 메서드라고 함
    - 이제 trait의 값이 바뀌었을 때, `traitCollectionDidChange` 메서드를 오버라이딩해서 쓰기보다는 다른 flexible한 API를 사용할 수 있다고함
- 우리가 만든 custom trait를 UIKit 뿐만 아니라, SwiftUI enviornment key로써도 활용 가능하다고 함
- 좀 더 자세한 내용은 `Unleash the UIKit trait system` 세션 참조

## Animated symbol images

- 이제 symbol image를 애니메이팅 시킬 수 있게됨.

<img width="1673" alt="스크린샷 2023-07-12 오전 9 42 45" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/f25fc863-a182-47cc-a493-78713c17b446">

- 요런 식으로 심볼 이펙트로 bounce를 주면 image가 bounce 하게 됨.

<img width="1690" alt="스크린샷 2023-07-12 오전 9 43 53" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/5f9dd600-6a9c-4a7b-bc3c-781a2b9fc3f1">

- 또한 심볼 이펙트를 준 다음에, 나중에 제거하는 것도 가능하다.
- 이 외에도 다양한 심볼 이펙트들이 있으니 궁금하면 아래 세션을 참고하자
    - `Animate symbols in your app`

## Empty states

- UIKit에서 여지껏 커스텀해야만 했던 empty state를 이제 공식적으로 지원하는 듯
- 어떠한 content를 display해줄 수 없을 때, 사용 가능한 `UIContentUnavailableConfiguration`이라는 구조체가 추가되었음
- `UIContentUnavailableConfiguration` 는 image나 text같은 placeholder content를 제공해줄 수 있음
    
    <img width="1730" alt="스크린샷 2023-07-12 오전 9 57 34" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/deaaded2-581d-4272-9020-fb3d52173fc5">


- 로딩상태도 만들어줄 수 있음
    
    <img width="1731" alt="스크린샷 2023-07-12 오전 9 58 52" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/d5d4b4a0-b5d1-45d3-a728-f018c7057c7c">

- 뷰컨트롤러의 contentUnavailableConfiguration을 업데이트 해줄 때, 활용하면 좋은 메서드는 새롭게 만들어진 `updateContentUnavailableConfiguration` 메서드.
    
    <img width="1760" alt="스크린샷 2023-07-12 오전 10 02 22" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/a77aed10-4cdd-48f5-bc55-db2e18e4ceae">

    - 위의 예시 코드를 보면, searchResult가 비어 있으면, .search() 상태로 만들어 주고 placeholder 이미지와 텍스트가 display되어지고 있음.
    - 만약 searchResult가 업데이트 되면, `setNeedsUpdateContentUnavailableConfiguration` 메서드를 호출하면 위의 `updateContentUnavailableConfiguration` 메서드가 호출되면서 업데이트 되는 형식.

# Internationalization

- 모든 애플 플랫폼에서 일관되고 고퀄리티의 경험을 주는 것은 필수적임. 그런데, 국가 별로 언어의 차이로 인해서 일관되지 못한 경험을 줄 수도 있기에 이런 점을 방지하기 위해, 업데이트를 하게 되었음.
- 특히 폰트와 텍스트 렌더링에서 업데이트를 하게 됨.
- 한 가지 예를 들어보면, line height에서 국가 별로 필요한 line height가 다른 상황이 생김.
    - 가령, 영어는 여유 공간이 상대적으로 적게 필요한데 반해,
        
        <img width="1203" alt="스크린샷 2023-07-12 오전 10 11 51" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/daa72d99-8095-46b4-873f-8811313529c2">

    - 반면, Arabic이나 Hindi, Thai 언어 같은 경우는 세로 공간이 더 필요함
        
        <img width="1630" alt="스크린샷 2023-07-12 오전 10 12 47" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/8f09566e-ab5e-41cd-998c-4565ad5c6c48">

    - 이런 상황이다 보니, 국가별 line-height를 dynamic하게 조절할 수 있도록 변경됨.
- 실제로 사용할 때는, 아래와 같이 새롭게 추가된 trait인 `typesettingLanguage` 프로퍼티를 활용하는 형식

<img width="1836" alt="스크린샷 2023-07-12 오전 10 15 24" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/491a9e6d-8845-4f86-9b23-bd080d71991f">

- 또 하나 추가된 점은 일부 symbol 이미지는 locale이 지원 됨
    
    <img width="1722" alt="스크린샷 2023-07-12 오전 10 16 58" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/03ba843a-4a86-4a50-bcd1-456baafdc191">

    <img width="1660" alt="스크린샷 2023-07-12 오전 10 17 29" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/30399d38-0def-4a11-91da-98cdc0fd2861">

    - 요런 식으로 사용해주면 됨.
    

# General enhancements

## Collection view improvements

- iOS 17에서 collection view의 퍼포먼스가 전반적으로 향상됨
    - sorting 동작을 수행할 때, iOS 16보다 약 두배가 빨라짐
    
    <img width="1686" alt="스크린샷 2023-07-12 오전 10 44 28" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/c5fb8ef0-7ebc-4991-8fa1-ed06109f0c04">

    - 만약 Diffable datasource의 snapshot을 활용하거나 collection view를 업데이트할 때, animation을 사용하지 않는다면 훨씬 더 빨라질 것임.

## Spring animation parameters

- Spring animation관련 springDuration과 bounce를 파라미터로 가지는 새로운 `animate` 오버로딩 메서드가 추가되었음
    - springDuration은 총 애니메이션 소요시간을 뜻하며, bounce는 얼마나 튕길지(0.0 ~ 1.0)를 결정하는 요소.
        
        <img width="1037" alt="스크린샷 2023-07-12 오전 11 09 48" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/6ec19cd6-1b6f-479f-90c5-584cc14f8a42">

        - 실제로 사용할 때는 이런 식.
    - 관련해서 좀 더 자세하게 알고 싶다면 아래 세션 참고.
        - `Animate with Springs`

## Default status bar style

- default status bar 스타일에도 변화가 생겼음.
- 기존 default status bar의 경우 앱이 다크 모드냐 라이트 모드냐에 따라서 스타일이 다르게 적용되었음.
- 그런데, 만약 앱의 어떤 콘텐츠가 검정색이나 하얀색 등 대비되는 색상을 가졌을 때, status bar를 식별하는데 다소 어려움이 존재했음
    
    <img width="1567" alt="스크린샷 2023-07-12 오후 8 27 05" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/ba39bc1b-8e29-48de-89b7-d27bab7ff6f7">

    - 위 예시의 앱은 라이트 모드를 쓰고 있으므로, default status bar 스타일은 검정색임을 확인할 수 있음.
    - 그런데 만약 콜렉션뷰의 첫번째 사진처럼, 사진이 어두울 경우 스크롤을 했을 때, 스테이터스 바가 제대로 식별되지 않는 이슈가 생길 수 있음.
        
        <img width="1230" alt="스크린샷 2023-07-12 오후 8 27 57" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/9f1faacb-4cd9-4768-914b-d9e889261c4e">

    - iOS 17부터는 위처럼 알아서 앱의 콘텐츠에 따라서 실시간으로 자동 조절해줌
    - 별도의 커스텀 코드를 작성해줄 필요가 없음. 그냥 default로 지정해주면 끝
