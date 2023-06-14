# Add Live Text interaction to your app

# 📌 What is Live Text?

애플이 iOS 버전을 16으로 올리면서 여러가지 기능이 추가되었다.

그 중에 하나가 바로 'Live Text' 기능이다.

이미지에 있는 텍스트를 뽑아주는 아주 편리한 기능이다.

- Text interactions
    
    <img width="269" alt="Untitled" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/acd5dc92-d3f4-4464-96ee-d9b090451b0d">

    - 텍스트를 선택해서 복사
- Translate
    
    <img width="265" alt="Untitled 1" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/56ec4937-f337-4e94-ae0c-98008f6392da">

- Data detection
    
    <img width="282" alt="Untitled 2" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/f1b9935d-ff6b-4f8f-9fcc-978837096d3a">

    
    - 주소 매핑
    - 전화 걸기
- QR codes
    
    <img width="267" alt="Untitled 3" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/97a810f5-f1b5-402d-84ad-454f17c96824">

    

Live Text는 위에 적은 4개의 기능을 모두 제공한다.

<img width="1000" alt="image" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/1481839d-782d-40a7-95f6-ba89585db84b">




# 📌 Live Text API

- 사용되는 곳은 이미지, 일시정지된 영상 프레임 (모두 '멈춰'있다는 점)
    - 실시간 카메라 스트림에서 텍스트를 분석해야한다면?
    Visition Kit의 데이터 스캐너 사용하면 됨 → 다른 세션 참고
- 이번 영상에서는 **정적인 이미지**에 Live Text를 적용하는 방법
- iOS 16부터 사용 가넝 & Apple Neural Engine, macOS 13.0를 가지는 모든 기기

## 어케 쓰나요?

<img width="511" alt="Untitled" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/046e9afb-2988-4aad-a615-d3ebfc93b429">


1. 먼저 분석할 이미지 준비
2. `ImageAnalyzer`에서 `ImageAnalysis` 이미지 분석기에서 이미지를 분석한다.
3. 분석이 완료되면 플랫폼에 따라서 `ImageAnalysisInteraction`, `ImageAnalysisOverlayView`에 전달된다.

크게 4개의 메인 클래스로 구성되어 있다. 아주 간단한 사용 흐름

# 📱 Adopting Live Text

- 지금 세션에서는 스크롤 뷰 안에 이미지 뷰가 있는 상황.
- 펜과 줌은 되지만 아무리 눌러도 텍스트 선택 안됨 → 아직 구현 안해서
- 이제 여기에 live text를 적용해보자

<img width="639" alt="d" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/68b7764f-fd19-437c-bbb0-f08fafa5f7b7">



1. `imageAnalyzer`와 `imageInteraction` 준비
2. 이미지뷰에 상호작용 추가


<img width="415" alt="스크린샷 2023-05-31 오후 2 37 18" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/487d0527-1f57-4b77-883a-00d255322446">



1. 새로운 이미지가 정해지면 인터랙션 타입과 분석을 재설정해준다
2. 왜냐면 이전 이미지의 분석값이 저장되어 있을 수도 있으니까

<img width="1085" alt="스크린샷 2023-05-31 오후 2 38 14" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/efcd45b5-bf8c-4f70-868f-d08cae996ace">


1. 현재 이미지를 분석하는 함수를 만듦
2. 이미지가 있으면 task 생성
3. 분석기를 생성 → 비동기로 이미지 분석
4. 중간에 앱 상태가 바뀌거나 이미지가 바뀌었을 수도 있으니 검사
5. 확인 결과 모두 통과면 분석 결과를 interaction에 저장

<img width="226" alt="스크린샷 2023-05-31 오후 2 51 58" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/3a193559-c0f2-427e-ad4f-3db5c7c8a6c8">
<img width="225" alt="스크린샷 2023-05-31 오후 2 52 35" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/38493d30-8886-42be-99aa-aa605e379707">
<img width="228" alt="스크린샷 2023-05-31 오후 2 49 38" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/8f6c79d5-8b43-4508-9723-c107fc7509ce">



# 📌 Tips and tricks

- 이제 실행하는 방법을 알았으니 몇가지 팁과 비결을 알려드림

## Preferred interaction types

```swift
import VisionKit
let interaction = ImageAnalysisInteracton()

// ...

interaction.preferredInteractionTypes = .**automatic**
```

- 기본적으로 텍스트 선택 제공
- **라이브 텍스트 버튼**을 누르면 감지된 데이터 항목들을 활성화함

<img width="427" alt="hjuy" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/430e7876-5b9a-408c-b2fa-a5cc6fc2b1c3">
<img width="424" alt="eqfqe" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/14db6e6b-b42d-46da-ad36-b5d903520860">



```swift
interaction.preferredInteractionType = .**textSelection**
```

- 데이터 감지가 없고 **텍스트 선택**만 있는거
- 아무리 라이브 텍스트 버튼을 눌러도 그대로
    
    
    <img width="401" alt="hjuh" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/ea5fd741-c869-4252-a941-81ffc04b9dbb">


```swift
interaction.preferredInteractionType = .dataDetectors
```

- 텍스트 선택 없이 데이터 감지기만 갖추는게 합리적이면
- 이 모드에서는 텍스트 선택이 불가해서 **라이브 텍스트 버튼**이 없음
- 진짜 데이터 감지만 됨. 밑줄 그어진 부분!
    
 <img width="395" alt="frde" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/29a68fbf-21ff-433f-8c83-1dd3446eaa93">


```swift
interaction.preferredInteractionTypes = []
```

- 이렇게 빈 배열을 넣어버리면 아예 상호작용이 불가함

```swift
interaction.allowLongPressForDataDetectorsInTextMode = false
```

- `.textSelection` 이나 `.automatic` 모드에서 꾹 누르면 데이터 탐지기 활성화 가능한 프로퍼티도 있음

## Supplementary Interface

- 라이브 텍스트 버튼
    <img width="426" alt="jhm" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/5b659609-2720-46d7-9464-3ea2aa2ca456">

    
    
- 퀵 액션
    
    <img width="394" alt="ghfb" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/79a2b0bd-6373-4720-9e43-874af55319fc">

    
    - 분석에서 나온 모든 데이터 감지
    - 라이브 텍스트 버튼을 작동하면 보임
- 커스텀?

```swift
interaction.isSupplementaryInterfaceHidden = false
```

- isSupplementaryInterfaceHidden 프로퍼티를 통해서 라이브 텍스트 버튼이랑 퀵 액션 숨길 수 있음

```swift
interaction.supplementaryInterfaceContentInsets = UIEdgeInsets(top: 0, left: 12, bottom: 18, right: 12)
```

- 밑에 인터페이스가 원래 우리의 UI 요소와 겹칠 수도 있으니까 콘텐츠 인셋 조절도 가능


<img width="403" alt="defrs'" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/a7ac4669-e0b9-4217-9373-1a316f32342e">



```swift
interaction.supplementaryInterfaceFont = UIFont.init(name: "폰트 이름", size: 0)
```

- 라이브 버튼과 퀵 액션, 그리고 명시된 텍스트 폰트가 변경됨
- 주의할 점은 버튼 크기 지정 일관성을 위해서 라이브 텍스트는 포인트 사이즈를 무시함!

# Match highlights to content

- `UIImageview`를 사용하지 않으면 하이라이트와 이미지가 일치하지 않음
→ `UIImageView`를 써야 일치한다
- 그 이유는 `UIImageView`의 경우 VisionKit이 ContentMode 속성을 써서
우리를 위해 cotentRect를 자동으로 계산해주기때문

# Interaction placement

- 이미지뷰가 스크롤뷰 내에 있다면 상호작용을 scrollview에 적용할 수도 있지만, 관리하기 힘들어서 비추
- scrollView는 계속해서 contentsRect가 변하기 때문에
- **상호작용을 주관하는 이미지 뷰**에 interaction을 넣는것을 적극권장

## Handling gesture conflicts

- 라이브 텍스트는 제스처로 상호작용하는데 제스쳐 내에서 **충돌이 일어날 수 있다**
- 어떻게 바로 잡을 수 있을까?

- `interactionShouldBeginAtPointForInteractionType` 델리게이트 메소드 사용
    
    - `false` 일 경우 해당 동작은 수행되지 않음
    - **포인트에 인터렉션 아이템이 있거나, 작동하는 텍스트 선택이 있을 때**만 동작 수행
    - 여기서 텍스트 선택을 확인하는 이유는 텍스트를 벗어난 곳을 누르면 deselect 해주기 위해서

---

- 제스처에 반응하지 않는다고 느낄 때
    - 이미 제스쳐 레커그나이저가 일을 처리하거나 스크롤, 줌 됐을 때 그럴수도 있음
    - 이럴 땐 `gestureRecognizer`의`gestureRecognizerShouldBegin`  델리게이트 메소드를 사용해서 해결할 수 이씀
    - 우선 `gestureRecognizer`의 위치를 윈도우 좌표 공간으로 전환 시킴?
    → 스크롤 뷰 안에 줌 되어 있을 때 고려
    - **!(포인트에 인터렉션 아이템이 있거나, 작동하는 텍스트 선택이 있을 때)** 긍까 둘다 아닐때 인식을 시작한다고 합니다 → ??
    

---

- UIView의 hitTest를 중단시키는 방법
    - hitTest?
    터치 이벤트를 수신해야 하는 사용자의 손가락 아래 가장 앞쪽 UIView 를 결정하기 위해 Hit-testing를 사용
    - 이전과 동일한 확인을 한 후에만 적절한 뷰를 돌려줘서 인터랙션이 가능하도록
        
# 라이브 텍스트를 사용하기 전에 지켜야할 점

- 하나의 이미지분석기만 사용하기
- 이미지 전환 최소화하기
- 시스템 자원을 최소화하기 위해 이미지가 나타나는 순간이나 그 전에 분석하도록
- 스크롤뷰일경우 스크롤 끝나고나서 분석
