z# Your guide to keyboard layout

iOS 15 버전 이전에 Keyboard를 관리하기 위해서는

<img width="1105" alt="Managing the keyboard" src="https://user-images.githubusercontent.com/53509789/203751026-103a76b5-a669-42a0-9dbd-1428ccc135c7.png">

다음과 같이 `Notification 등록, Notification에서 적절한 offset과 animation을 도출 및 Do something, layout 조정` 코드를 모두 작성해야한다.  

<img width="1622" alt="스크린샷 2022-11-24 오후 2 55 12" src="https://user-images.githubusercontent.com/53509789/203751040-bf7500bf-345b-48f7-bd83-9a0f2b3025bd.png">


iOS 15에서 나온 brand new API !  
autolayout에 추가된 **`UIKeyboardLayoutGuide`** 을 통해 키보드에 제약조건을 걸 수 있다.

`UIKeyboardLayoutGuide`을 통해 위의 코드를 변경해보자.
- `Notification` 등록 불필요
- `response` 함수 불필요
- 키보드 레이아웃 -> `KeyboardLayoutGuide` 로 변경  

위의 모든 코드는 이 한 줄로 요약이 가능.  
<img width="1595" alt="스크린샷 2022-11-24 오후 2 59 50" src="https://user-images.githubusercontent.com/53509789/203751058-315dbbb0-c085-4729-9854-884bc12e1bf6.png">  


## Updating to KeyboardLayoutGuide

<img width="1014" alt="Updating to keyboard layout guide" src="https://user-images.githubusercontent.com/53509789/203751076-2b422e20-8336-46bc-8cf7-f7a6f9cb4e56.png">

- 키보드의 높이는 다양(자동완성, 이모지.. 등) -> 표시되는 크기에 맞게 조정됨
- 키보드가 해제될때 Safe area 하단에 걸림

## `UIKeyboardLayoutGuide`의 속성: `followsUndockedKeyboard`
키보드가 도킹 해제되거나 떠 있는 경우에도 모든 키보드 앵커를 따르려면 `followUndockedKeyboard`를 `true`로 설정. (`default: false`)

해당 속성에 대해 알기 전, `UITrackingLayoutGuide` 부터 알아야한다.

## `UITrackingLayoutGuide`
**화면 가장자리 주위를 이동할때 변경하려는 제약조건을 추적하는 레이아웃 가이드**

```
특정 가장자리 근처에 있을 때 활성화되고 멀어질때 비활성화되는 제약 조건
특정 가장자리 근처에 있을 때 비활성화되고 멀어질때 활성화되는 제약 조건
```
을 지정하여 원하는 레이아웃 만들기 !

<img width="500" height="400" alt="스크린샷 2022-11-24 오후 4 30 19" src="https://user-images.githubusercontent.com/53509789/203760345-bf990d31-366d-42e0-8810-012e21826795.png"><img width="500"  height="400" alt="스크린샷 2022-11-24 오후 4 30 32" src="https://user-images.githubusercontent.com/53509789/203760351-b1f9e70c-3127-4f3f-8a7d-10d118ad8045.png"><img width="500"  height="400" alt="스크린샷 2022-11-24 오후 4 31 32" src="https://user-images.githubusercontent.com/53509789/203760354-6ff1738b-ab05-46b5-8a5f-025b3852bbbc.png">

1. 도킹된 키보드는 하단 근처(near)에 있고 다른 가장자리에서 떨어져 있는 것(awayFrom)으로 간주됩니다. 
2. 도킹 해제된 키보드는 모든 가장자리에서 떨어져 있거나 어느 가장 자리 근처에 있을 수 있다.
3. 플로팅 키보드의 경우 모든 가장자리에서 근처에 있거나 떨어져 있을 수 있으며 동시에 두개의 인접한 가장자리 근처에 있을 수 있다.  
  

다음과 같이 UI Components들이 있는 씬의 예시를 살펴보자.  
<img width="1565" alt="스크린샷 2022-11-24 오후 4 08 05" src="https://user-images.githubusercontent.com/53509789/203751094-722293a9-94da-4189-805f-6428b3e32d23.png">

`UIKeyboardLayoutGuide`는 또 다른 새 레이아웃 가이드인 `UITrackingLayoutGuide`의 하위 클래스이므로  
`UIKeyboardLayoutGuide`를 이용하여 다음과 같이 레이아웃을 설정할 수 있다.

``` swift
// 키보드가 도킹 해제되거나 떠 있는 경우에도 제약 조건을 따라라.
view.keyboardLayoutGuide.followUndockedKeyboard = true
```
<img width="1423" alt="스크린샷 2022-11-24 오후 4 08 12" src="https://user-images.githubusercontent.com/53509789/203751125-d1d39274-cbce-483a-883e-a41185e02631.png">

키보드가 화면의 top과 멀어져있을때는 키보드 위에 editView를 위치시키고  
키보드가 화면의 top과 가까진다면 editView를 화면의 bottom 위에 위치시킨다.  

<img width="1524" alt="let anayFromSides" src="https://user-images.githubusercontent.com/53509789/203751141-e87d5963-844d-4107-9400-4e51853d6c6e.png">

키보드가 화면의 leading, trailing에서 멀어진다면 editView와 imageView를 키보드 중앙에 위치시킨다.  
키보드가 화면의 trailing과 가까워진다면 editView를 화면의 trailing에 맞추고, imageView를 키보드 중앙에서 반대 쪽으로 이동시켜 leading과 맞춘다.  
키보드가 화면의 leading과 가까워진다면 editView를 화면의 leading에 맞추고, imageView를 키보드 중앙에서 반대 쪽으로 이동시켜 trailing과 맞춘다.
  

위의 코드들을 통해 다음과 같이 레이아웃을 설정할 수 있다.  
![1](https://user-images.githubusercontent.com/53509789/203770477-fccf3526-1fbc-4e75-bca2-7a8164088768.gif)
  
  
# Use the camera for keyboard input in your app
iOS 15 부터는 카메라를 사용하여 텍스트 입력을 받을 수 있다.  
이때, 모든 카메라에 인식된 모든 텍스트를 가져오는 것이 아닌, 원하는 컨텐츠만 받기 위해 컨텐츠 필터링을 할 수 있다.  

필터링은 `UITextContentType`과 `UIKeyboardType`을 통해 수행된다. 

<img width="1539" alt="스크린샷 2022-11-24 오후 8 22 00" src="https://user-images.githubusercontent.com/53509789/203781509-58b1be1a-4072-4007-86c3-d10d3730e023.png">
 
카메라를 통해 아래 7개의 `UITextContentType`를 필터링 할 수 있다.  

<img width="743" alt="스크린샷 2022-11-24 오후 8 24 20" src="https://user-images.githubusercontent.com/53509789/203781499-067b9a0c-e62e-4467-89fe-dd88273af6e8.png">

예시를 살펴보자.

다음과 같이 각 컴포넌트에 맞게 `UITextContentType`과 `UIKeyboardType`를 설정한다. 또한 `autocorrectionType = no`를 통해 키보드에서 카메라에 빠르게 액세스할 수 있는 버튼을 만들어줍니다. 

<img width="1529" alt="스크린샷 2022-11-24 오후 8 27 36" src="https://user-images.githubusercontent.com/53509789/203781495-a484b079-803a-4e0e-85e7-6da1aa40a94d.png">

카메라 입력을 암시하는 UI 버튼을 생성하기 위해서는 전용 런처 버튼을 추가해야한다.  
이를 위해서는 먼저, iOS 15의 새로운 기능인 `UIAction`의 팩토리 메서드 `captureTextFromCamera`를 이용해야한다. 해당 함수를 통해 키보드 카메라를 시작할 수 있는 Action을 생성한다.

<img width="632" alt="스크린샷 2022-11-24 오후 8 33 53" src="https://user-images.githubusercontent.com/53509789/203781492-13889da2-8d5f-4680-8283-8e8ab5e26514.png">
<img width="720" alt="스크린샷 2022-11-24 오후 8 36 23" src="https://user-images.githubusercontent.com/53509789/203781488-5d8166da-2eb0-4fb4-b0bc-7f220be2feb4.png">

하지만 명심해야할 것이 하나 있다.

이러한 작업은 항상 사용 가능한 것이 아니다. 다음과 같은 전제 조건이 존재하고 지원 언어는 7가지 뿐이다.

<img width="713" alt="스크린샷 2022-11-24 오후 8 43 57" src="https://user-images.githubusercontent.com/53509789/203781484-c39d9c89-b5c9-4157-92ce-8436572d31e6.png"> 
<img width="694" alt="스크린샷 2022-11-24 오후 8 45 19" src="https://user-images.githubusercontent.com/53509789/203781475-09c2714a-a865-41d9-abfd-86a94c24c992.png">

따라서 `canPerformAction` 메서드를 이용하여 `captureTextFromCamera` 기능을 실행할 수 있는지 확인해야한다.

<img width="1524" alt="스크린샷 2022-11-24 오후 8 39 05" src="https://user-images.githubusercontent.com/53509789/203781486-69d6f878-bba5-4d76-a4d2-eb24beb9f47c.png">

  
  
마지막으로 카메라를 통해 텍스트를 캡처하여 Image에 넣는 예시를 살펴보자. 

<img width="266" height="390" alt="스크린샷 2022-11-24 오후 8 49 42" src="https://user-images.githubusercontent.com/53509789/203780333-4dc375ce-32e0-4733-b377-a91d14fd46cd.png"><img width="266" height="390" alt="스크린샷 2022-11-24 오후 8 49 48" src="https://user-images.githubusercontent.com/53509789/203780330-e8281dd1-1a24-45c1-8b4f-107ec5e2a1fe.png">

### Text Control
Text Control은 UIKeyInput의 확장인 UITextInput 프로토콜을 채택한다.

<img width="746" alt="스크린샷 2022-11-24 오후 8 50 54" src="https://user-images.githubusercontent.com/53509789/203780325-ff6bcf01-e292-410b-9fa1-20f26e49ce43.png">
<img width="746" alt="스크린샷 2022-11-24 오후 8 54 27" src="https://user-images.githubusercontent.com/53509789/203780320-4e9d225f-c39a-4a87-8be1-dc1b812e1eca.png">
<img width="746" alt="스크린샷 2022-11-24 오후 9 08 58" src="https://user-images.githubusercontent.com/53509789/203781126-0baf9afa-ba1a-41a7-851a-f80459a8506d.png">


* UIKeyInput 프로토콜
    * insertText 메서드를 통해 카메라에서 앱으로 텍스트를 전송한다.
* UITextInput 프로토콜
    * setMarkedText 메서드를 통해 삽입할 텍스트의 미리보기를 얻을 수 있다. 선택사항이기 때문에 사용하지 않으려면, UIKeyInput 프로토콜만 채택하면 된다.
* UITextInputTraits 프로토콜
    * 특정 컨텐츠에 대한 카메라 입력을 필터링 하는 데 사용되는 KeyboardType 및 TextContentType와 같은 선택적 속성으로 구성


따라서 카메라 input은 선택적으로 원하는 기능에 따라, 이러한 프로토콜들을 채택할 수 있다.  

ImageView에 카메라를 통해 텍스트를 캡처하여 넣기 위해 다음과 같이 코드를 구성하면 된다.

<img width="335" alt="스크린샷 2022-11-24 오후 9 02 12" src="https://user-images.githubusercontent.com/53509789/203780316-48944fca-6eb1-4654-b7e4-46daf169df4c.png"> <img width="937" alt="스크린샷 2022-11-24 오후 9 02 24" src="https://user-images.githubusercontent.com/53509789/203780309-e9d0780e-3f09-46d3-b0da-005c6a6cee3a.png">
