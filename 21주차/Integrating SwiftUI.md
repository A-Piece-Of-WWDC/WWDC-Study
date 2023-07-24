# Integrating SwiftUI

- SwiftUI뷰들을 어떻게 효과적으로 현재 우리의 앱에 적용시킬지에 대해 소개해주는 세션

## Hosting SwiftUI views in your app

- SwiftUI로 만들어진 뷰를 UIKit기반으로 만들어진 우리의 앱에 적용시키는거는 매우 쉬움
    
    <img width="1656" alt="스크린샷 2023-06-27 오후 10 10 51" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/6616f410-3073-4432-8686-1878f2e11d88">

    - UIKit의 경우, UIHostingController를 이용하여 SwiftUI뷰를 감싸주면 된다
        
        <img width="1842" alt="스크린샷 2023-06-27 오후 10 11 44" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/d67ac5aa-fde6-4424-8858-6aa039b40228">

    - UIHostingController는 UIViewController의 subclass로서, rootView라는 SwiftUI뷰를 파라미터로 가지고 있다
    - Storyboard를 통해서도 만들어줄 수 있음
        
        <img width="701" alt="스크린샷 2023-06-27 오후 10 16 15" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/e05e8553-e336-48b8-a891-970a78a4db2f">

        <img width="384" alt="스크린샷 2023-06-27 오후 10 16 30" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/19031eac-6c78-4bec-826b-2b7e1549d37b">


- 만약 스토리보드를 통해 UIHostingController를 만들 경우, 아래 스크린샷 처럼, segue를 드래그한 뒤, 코드에 드랍해주면 다음과 같이 간단하게 UIHostingController를 생성하는 코드가 만들어진다.

<img width="1839" alt="스크린샷 2023-06-27 오후 10 19 51" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/15452708-1cb5-44e2-84bd-83206e48b6aa">

- 이후, 어떤 SwiftUI뷰를 보여줄지 rootView로 설정한 뒤에 파라미터로 넘겨주면 끝

<img width="1835" alt="스크린샷 2023-06-27 오후 10 22 09" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/f20da00c-bfbb-4f1a-b2fc-a5a1ee29020c">

## Embedding existing views in SwiftUI

- 우리가 만든 SwiftUI 뷰를 UIKit 생태계에서 활용하는 것은 위의 내용처럼, UIHostingController를 활용하면 된다. 그렇다면 반대로 이미 만들어진 Custom UIView를 SwiftUI에서 쓸 순 없을까.
- Representable Protocol을 활용한다면, 가능하다
    
    <img width="1572" alt="스크린샷 2023-06-27 오후 10 31 06" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/5f301d7e-ca6a-451c-a2af-e0ae3ed62c56">

    - UIView는 UIViewRepresentable 프로토콜, UIViewController는 UIViewControllerRepresentable 프로토콜을 활용해주면 된다.

### Representable Protocol

<img width="1356" alt="스크린샷 2023-06-27 오후 10 40 12" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/1b82da5c-3c4a-470e-9fcd-7b3e964fd8ec">

- Representable 프로토콜의 메서드는 크게 3종류로 이루어져 있음.
    - Make View/Controller (Required)
        - 처음 뷰가 만들어질 때, 최초 1번 호출
    - Update View/Controller (Required)
        - Make 메서드가 호출된 직후에 호출됨.
        - SwiftUI의 뷰 업데이트 요청에 의해 수시로 호출될 수 있음
    - Dismantle View/Controller (optional)
        - 뷰가 remove될 때 호출
        - clean up 해야하는 코드들이 있을 때, 여기에 넣어주자

- 각각의 Representable 프로토콜은 아래와 같다.

<img width="1838" alt="스크린샷 2023-06-27 오후 10 45 57" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/2325779f-bdf4-4325-a12d-1a39e7eaae44">

## Advanced Integration of Views

- 단순히 SwiftUI뷰를 UIKit에서 띄우거나, UIKit의 뷰를 SwiftUI에서 띄워주는 것을 넘어서, 우리는 더 다양한 액션을 하고 싶을 수 있음.
    - 예를 들어, SwiftUI의 뷰에서 일어난 버튼 액션을 UIKit의 뷰로 전달한다거나 또는 그 반대의 경우가 필요할 수 있다.
    - 이를 가능해주기 위해, 만들어진게 바로 **Representable Context**

### Representable Context

<img width="1660" alt="스크린샷 2023-06-27 오후 11 04 41" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/b0d92601-c353-45a1-988f-49879938cf74">

- Representable Context 구조체는 크게 3가지 프로퍼티로 이루어져 있다.
    - Coordinator
        - 코디네이터는 UIView와 SwiftUI뷰가 긴밀하게 서로 연결되게끔 도와주는 역할을 수행한다.
            - 쉽게 말해서, delegate같은 역할
    - Enviornment
        - 뷰의 환경과 관련된 프로퍼티이다. color scheme이나 sizeClass 등이 있다.
    - Transaction
        - SwiftUI의 애니메이션과 관련된 프로퍼티.

<img width="1748" alt="스크린샷 2023-06-27 오후 11 20 41" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/8e592053-cd88-4518-af0a-629053e720c4">

- 다시 아까전에 봤던 diagram으로 돌아오면, 뷰를 make하고 update하는 과정에서 파라미터로 Representable Context를 넘겨준다.
- 파라미터로 넘겨진 Context에는 enviornment와 transaction과 관련된 정보들이 담겨져 있음
- 만약 코디네이터를 활용하고 싶다면 직접 만들어줘야한다
    - 코디네이터는 뷰가 make되기 전에 먼저 호출됨
- 조금 복잡할 수 있는데 예시를 통해 보자.

<img width="1708" alt="스크린샷 2023-06-27 오후 11 54 08" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/e8854975-d5f0-4995-9b11-4eb7658c8f6e">

<img width="1917" alt="스크린샷 2023-06-27 오후 11 54 50" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/47edbaad-4a8c-443a-8c99-33537020390a">

- 코드를 설명하기에 앞서, 예제에서 하고자 하는 목표는 다음과 같다.
    - UIKit으로 만들어진 RatingsControl 뷰가 있다
    - RatingsControl을 SwiftUI 뷰로 활용하기 위해 UIViewRepresentable 프로토콜을 준수하는 RatingsControlRepresentation을 만들어줬다.
    - RatingsControl 우측에는 현재 별점을 표시해주기 위한 SwiftUI Text 뷰가 있다.
    - 우리는, UIKit으로 만들어진 RatingsControl에서 일어난 탭 이벤트로 들어온 rating 수치를 SwiftUI Text에 전달하려고 한다.
- UIKit에서 SwiftUI로 이벤트를 전달해주기 위해 코디네이터를 만들어줬다.
- UIKit에서 변화하는 rating수치를 SwiftUI로 전달해주기 위해 코디네이터에서 rating프로퍼티를 @Binding으로 묶어두고 있다.

- 방금 작성한 코드를 SwiftUI뷰에서는 아래와 같이 활용되는 것을 확인할 수 있다.

<img width="1785" alt="스크린샷 2023-06-28 오전 12 12 31" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/099f027c-a4f3-4501-b39f-880b5e001bb7">

## Integrating your data model

- 어떤 DataModel에 의존해서 SwiftUI뷰를 그린다고 해보자.
- 이 때, DataModel은 유저에 의해서든 시간 흐름에 의해서든 수시로 변화할 수 있음. 하지만 SwiftUI 프레임워크는 DataModel이 변화한 사실을 알지 못함.

<img width="1685" alt="스크린샷 2023-06-28 오후 6 42 42" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/5a75d257-6a9b-41d3-8b65-16e3bdad4096">

- 이를 해결하기 위해 등장한게 BindableObject 프로토콜(현재는 **ObservableObject**로 리네이밍됨)
- 해당 DataModel을 SwiftUI에서 쓸 때는 @ObjcetBinding 프로퍼티 래퍼를 사용해주면 된다(현재는 **@ObservedObject**로 리네이밍 됨
- SwiftUI의 뷰는 didChange 퍼블리셔(현재는 **objectWillChange**)를 구독하고 있다가 dataModel이 변화하면 퍼블리셔를 통해 이벤트를 방출하고 구독하고 있던 SwiftUI의 뷰가 다시 그려지는 형태로 동작하게됨
- DataModel에는 데이터가 수시로 변화하고 변할 때 새로 그려줘야 하는 프로퍼티를 @Published 프로퍼티로 감싸서 관리해주면 된다.
<img width="771" alt="스크린샷 2023-06-28 오후 7 02 32" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/7b148542-1a45-4363-89b5-cdff952920ab">
