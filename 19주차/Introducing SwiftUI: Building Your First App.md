# SwiftUI Basic Concept

## View Protocol

- SwiftUI에서 쓰이는 모든 뷰들은 오직 View 프로토콜만을 따르고 있음
    - UIView 클래스를 상속받는 UIKit에서 사용하는 뷰들과 차이점이 존재
    - 즉 이는 SwiftUI에서 쓰이는 모든 뷰들은 그 어떤 프로퍼티나 메서드들도 상속받지 않고 있음
- 또한 클래스가 아닌 구조체를 활용하고 있음
- 구조체를 사용하므로 Stack영역에 할당되게 됨
- 값타입을 사용하게 되므로 SwiftUI의 View들은 extremely lightweight 함.
    - 즉, 우리는 SwiftUI코드를 최적화하기 위해 리팩토링을 하는 등의 과정이 불필요 하게됨
- View 프로토콜은 오직 하나의 프로퍼티만을 필요로함
    - **body**
- SwiftUI의 View는 각기 다른 조각 조각의 View들이 합쳐져서 만들어지는 형태
    - body 안에 선언된 Image, Text, CustomView 등등의 조합된 형태

# Reactive UI

- SwiftUI는 기본적으로 반응형 UI를 가능하게 해주는 UI 프레임워크
- 반응형 UI를 가능하게끔 만들어주는 프로퍼티 래퍼들이 존재함

## @State

![스크린샷 2023-05-24 오후 9 02 26](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/0f62f1bc-d655-4198-9ade-121ba1e61548)

![스크린샷 2023-05-24 오후 9 14 09](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/25db0f13-eab8-4b7d-93f7-7e6cf868b54a)

- 위 이미지에서 zoomed 프로퍼티는 이미지가 줌이 되었는지 안되었는지를 판단하는 프로퍼티로, @State 래퍼로 감싸져있음
- @State 프로퍼티는 값이 변하면 SwiftUI 프레임워크는 body에게 뷰를 새로운 state value를 사용해서 다시 그리라고 요청함

## Managing Dependencies Is Hard

![스크린샷 2023-05-24 오후 9 29 06](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/f7b84464-4c2c-4de8-932e-f0a008eb5b23)

- 복잡한 의존성 구조로 이루어진 각각의 컴포넌트들로 인해서 상당히 힘든 디버깅

![스크린샷 2023-05-24 오후 9 31 36](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/f8df20e4-8269-4c7d-ba64-c11e08ce280b)

![스크린샷 2023-05-24 오후 9 36 12](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/781abd32-ba1f-4efa-9f83-b214ee020e60)

- 앞서 보았던 줌인, 줌아웃 뷰의 UIKit 버전. 상당히 복잡하고 번거로운 기능들을 수행해아함
- 유저가 어떤 동작을 먼저하느냐에 따라서 24가지 case(zoomIn, zoomOut, enhance, completion의 조합)가 존재할 수 있음. 휴먼 에러 발생 가능성도 높음
    
    ![스크린샷 2023-05-24 오후 9 39 09](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/ab0c189f-6b14-4d71-be9d-785825018b04)

- 24가지 뿐만아니라 동일 버튼을 여러번 누르는 유저도 존재할 수 있음.
    
    ![스크린샷 2023-05-24 오후 9 42 27](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/86361238-58e2-4026-a5b9-7a43b355c0e9)

- 뷰가 많아지면 많아질 수록, 다양한 뷰의 State가 나오게 되고 이는 코드의 복잡성을 기하급수적으로 상승시킴
    - delegate 메서드, 타겟 액션, 노티피케이션, 컴플레션 핸들러 등등이 점점 많아지게 됨.
        
        ![스크린샷 2023-05-24 오후 9 43 52](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/87598209/72122d5d-b807-4b5e-86ba-9474997b7021)

- 인간이 한 번에 생각할 수 있는 양은 정해져 있음. 그 이상의 복잡도를 넘어가면, 이는 **Bug**의 발생을 의미
- 이러한 Pain Point를 해결하기 위해 SwiftUI의 등장
