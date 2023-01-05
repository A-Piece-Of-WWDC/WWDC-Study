# What's new in UIKit (WWDC21)

## Productivity
이 부분은 iPadOS의 핵심이다. 
iOS15에서는 iPad 멀티 태스킹, iPad 포인터, 키보드 단축키 키보드 Navigation이 업데이트 되었다.
### iPad Multi Tasking
<img width="1011" alt="스크린샷 2023-01-04 오후 10 01 59" src="https://user-images.githubusercontent.com/53509789/210571792-6899145d-1c02-4916-8e56-82d6680651c9.png">
길게 눌렀을 때 컨텍스트 메뉴에 Open in New Window 버튼이 생성되었다. 해당 버튼을 통해 새로운 창으로 열 수 있다.
drag&drop 혹은 MultiTasking을 이용하여 Split 뷰를 이용할 수 있다.
<img width="993" alt="스크린샷 2023-01-04 오후 10 02 17" src="https://user-images.githubusercontent.com/53509789/210571799-082c7437-de74-4e85-ad29-2b54a47e6d0e.png">
또한 스와이프 다운하여 메뉴(창 선반)에 놓을 수도 있다.

<img width="1427" alt="스크린샷 2023-01-04 오후 10 03 05" src="https://user-images.githubusercontent.com/53509789/210571809-2b1ac4ae-b30f-4a68-be73-3c48ff94cbb6.png">
다음과 같이 UIWindowScene의 ActivationAction에 Activity를 추가하여 만들면 된다.

### iPad Pointer
- Band selection
<img width="1290" alt="스크린샷 2023-01-04 오후 10 06 49" src="https://user-images.githubusercontent.com/53509789/210571811-2291acbb-e9a1-4e49-af25-396f67367a8d.png">
Band selection 기능이 추가되었고 이를 이용하여 드래그를 통해 여러 개를 한번에 선택할 수 있다.

- Pointer accessories
<img width="1259" alt="스크린샷 2023-01-04 오후 10 07 04" src="https://user-images.githubusercontent.com/53509789/210571814-173ff622-4223-4d4e-afd5-a353539ebea3.png">
또한 위와 같은 포인터 악세사리가 추가되어 컨텍스트에 맞게 적용할 수 있게 되었다.

### 키보드 단축키
<img width="985" alt="스크린샷 2023-01-04 오후 10 08 00" src="https://user-images.githubusercontent.com/53509789/210785696-f746b84a-97d0-467e-9d69-f8b3eed9dd5e.png">
단축키가 분류되었고 내장 검색 기능이 추가되어 원하는 단축키를 찾기 쉬워졌으며 이를 통해 다양한 기능을 쉽게 사용할 수 있다.

### 키보드 Navigation
<img width="1013" alt="스크린샷 2023-01-04 오후 10 05 00" src="https://user-images.githubusercontent.com/53509789/210786826-0eacb723-1fcc-4b80-b7c7-e170760cfb43.png">
- UIFocusSystem을 모든 플랫폼(iPhone 제외)에서 사용 가능하게 만들었다. 키보드의 화살표키를 사용하여 포커스 항목 간의 이동이 가능해졌고 탭 키를 통해 포커스 그룹 이동이 가능해졌다.

### drag & drop
<img width="484" alt="스크린샷 2023-01-04 오후 10 06 47" src="https://user-images.githubusercontent.com/53509789/210787090-6b939f1a-1cac-41ed-8104-65285fdcb63d.png"><img width="416" alt="스크린샷 2023-01-04 오후 10 07 09" src="https://user-images.githubusercontent.com/53509789/210787060-86db52c9-ed23-4c63-8042-5253e201e6c5.png">

같은 앱 내에서 사용되던 기능이지만, iOS15에서는 다른 앱에서 드래그로 사진을 가져와서 드롭할 수 있게 변경되었다.

## UI refinements

### UIToolbar UITabBar 개선
<img width="1295" alt="스크린샷 2023-01-04 오후 10 13 52" src="https://user-images.githubusercontent.com/53509789/210571838-a86162fe-ff51-40e6-9a27-127df4d45b45.png">
아래로 스크롤 시 배경을 지워서 내용을 더 명확하게, 친화적으로 보여지게 변경되었다. 

<img width="1468" alt="스크린샷 2023-01-04 오후 10 14 03" src="https://user-images.githubusercontent.com/53509789/210571844-70660a85-fd00-4ba4-b422-8cc54e95a547.png">
다음과 같이 코드를 작성한다면 원했던 결과가 나오지 않는다.
<img width="1427" alt="스크린샷 2023-01-04 오후 10 14 32" src="https://user-images.githubusercontent.com/53509789/210571848-a5b01729-edd3-4b57-87f4-f2085ba9fd1d.png">
따라서 다음과 같이 UITabBarAppearance를 생성하고 이를 할당해주어야한다.
이는 원래 네비게이션 바에서만 사용했지만 UIToolbar, UITabBar에서도 기능이 추가되었다.


### List 개선
<img width="1469" alt="스크린샷 2023-01-04 오후 10 20 59" src="https://user-images.githubusercontent.com/53509789/210571849-f994fcc0-879f-4585-8c14-9ae593d582fd.png">
* Header 개선
  * Plain 리스트에서 섹션 헤더는 콘텐츠에 맞추어 더 잘 어울리게 표시되었고 스크롤 시 위에 고정될때만 보이는 배경 material만 표시된다. 또한 섹션들을 구분하기 위해 섹션 헤더 위에 패딩이 추가되었다.
<img width="329" alt="스크린샷 2023-01-04 오후 10 21 16" src="https://user-images.githubusercontent.com/53509789/210571855-0c248612-c0e8-46f5-885a-8c690c389f48.png">
* .prominentInsetGrouped 타입 추가
  * 아이패드의 Sidebar list와 유사하며 그것의 컴팩트한 버전이다. 시계 앱의 알람 탭이 해당 스타일을 잘 활용하는 사례이다.
<img width="385" alt="스크린샷 2023-01-04 오후 10 21 49" src="https://user-images.githubusercontent.com/53509789/210571856-e78bbc8d-38c1-4f82-9865-e936b7dda1df.png">
* .extraProminentInsetGrouped 타입 추가
  * 헤더가 계층 구조를 유지하며 시각적으로 풍부한 컨텐츠에 사용할 수 있는 스타일이다. 워치 앱의 Face Gallery가 해당 스타일을 잘 활용하는 사례이다.

추가된 리스트들을 잘 활용하려면 iOS14에서 나온 UIListConfiguration API를 사용하면 된다!
<img width="1421" alt="스크린샷 2023-01-04 오후 10 23 06" src="https://user-images.githubusercontent.com/53509789/210571862-1b58c7d4-a178-4d7c-9e93-971e6704a798.png">

### Half Height Sheet
<img width="422" alt="스크린샷 2023-01-04 오후 10 24 01" src="https://user-images.githubusercontent.com/53509789/210571865-fbe41951-8c83-448e-bc60-45ebbd8fe94e.png">
절반 높이의 시트가 추가되었다.

### UIDatePicker
<img width="746" alt="스크린샷 2023-01-04 오후 10 25 40" src="https://user-images.githubusercontent.com/53509789/210571867-00f44746-52f2-4d47-81ad-47fc26e327f7.png">
iOS14에서 달력으로 변경된 UIDatePicker가 다시 Wheel로 돌아왔다. 또한 직접 입력하는 기능이 추가되었다.

## API enhancements
### UIButton API
<img width="702" alt="스크린샷 2023-01-04 오후 10 26 11" src="https://user-images.githubusercontent.com/53509789/210571870-6075c474-b888-4a84-b1f9-e34efcf7d964.png">

- 배경
  - 배경이 없던 Plain 버튼에 gray 배경 추가
  - Tinted 버튼 또한 배경 추가
  - Filled 버튼은 불투명한 배경 추가
- 버튼 텍스트에 Multi Line 적용 가능
<img width="509" alt="스크린샷 2023-01-04 오후 10 28 52" src="https://user-images.githubusercontent.com/53509789/210571873-86c17597-b834-414c-aeb7-1d80a123d146.png"><img width="472" alt="스크린샷 2023-01-04 오후 10 29 24" src="https://user-images.githubusercontent.com/53509789/210571876-865a8f43-23fa-4131-bd6a-537a7bcba2d0.png">
- Pull-down, Pop-up 버튼
<img width="1298" alt="스크린샷 2023-01-04 오후 10 32 05" src="https://user-images.githubusercontent.com/53509789/210571882-ad202c84-74c8-4344-8d86-6ba1934e4edc.png"><img width="500" alt="스크린샷 2023-01-04 오후 10 32 21" src="https://user-images.githubusercontent.com/53509789/210571885-e95d18e5-d1ed-4a26-adf9-6cbd9ff62e8a.png">
  - UIMenu의 서브 메뉴에서 메뉴의 계층 설정 가능 (뎁스를 가지는 버튼)
 
<img width="1346" alt="스크린샷 2023-01-04 오후 10 31 06" src="https://user-images.githubusercontent.com/53509789/210571878-2a4241f2-f722-4afe-9b46-d46eb2983c3b.png">

### SF Symbol
<img width="1385" alt="스크린샷 2023-01-04 오후 10 32 49" src="https://user-images.githubusercontent.com/53509789/210571889-90d47104-e367-460b-9d65-1e495c99b1f5.png">
<img width="1389" alt="스크린샷 2023-01-04 오후 10 36 15" src="https://user-images.githubusercontent.com/53509789/210571896-76e642b9-829a-4f14-899b-e1d89f20d62c.png">
- 원하는 부분만 색상 변경 가능, 색 조합 가능

### Content size category limits
<img width="1382" alt="스크린샷 2023-01-04 오후 10 37 08" src="https://user-images.githubusercontent.com/53509789/210571898-b533ac41-666f-49d1-9836-1496bffd114c.png">
콘텐츠에 Maximum, Minimum을 제한 설정가능

### UIColor
<img width="831" alt="스크린샷 2023-01-04 오후 10 45 41" src="https://user-images.githubusercontent.com/53509789/210571906-7b787372-a464-4178-8f0c-6160156e5339.png">
모든 플랫폼에서 시스템 색상을 통합하였고 일부 프레임워크에서만 사용가능했던 색상을 이제는 UIKit에서 사용할 수 있다.

### Color picker
<img width="1363" alt="스크린샷 2023-01-04 오후 10 46 43" src="https://user-images.githubusercontent.com/53509789/210571912-431f3e10-f23e-4c94-a2e6-1278d0525d5b.png">
- 정교하게 색상을 선택하여 선택이 완료됐을 때 UI를 업데이트할 수 있다.

### TextKit2
<img width="914" alt="스크린샷 2023-01-04 오후 10 48 12" src="https://user-images.githubusercontent.com/53509789/210571915-63733a83-cae9-425b-a104-be41670f352c.png">
- iOS, iPadOS, tvOS, macOS에서 사용할 수 있는 새로운 텍스트 레이아웃 시스템

### Scene level sharing
<img width="891" alt="스크린샷 2023-01-04 오후 10 51 05" src="https://user-images.githubusercontent.com/53509789/210571920-18b8c668-0d04-4865-8689-4bb3725c1c6c.png">
화면에서 공유할 수 있는 콘텐츠를 나타낼 수 있다.
iOS의 새로운 시리 기능인 Share This 기능과 Mac의 NSSharingServicePicker ToolbarItem에서 사용할 수 있다.

### Cell Configuration
<img width="806" alt="스크린샷 2023-01-04 오후 10 51 48" src="https://user-images.githubusercontent.com/53509789/210571921-11dd3343-5873-4f2c-8efd-1dc40ca30e12.png">
<img width="1349" alt="스크린샷 2023-01-04 오후 10 52 42" src="https://user-images.githubusercontent.com/53509789/210571926-fe3665f7-8016-42af-8439-e8caa6a2a8e8.png">
셀의 하위 클래스들을 만들고 재정의하는 작업 대신, iOS15의 새로운 클로저(configurationUpdateHandler) 를 기반으로 셀을 구성할 수 있다.

<img width="967" alt="스크린샷 2023-01-04 오후 10 53 30" src="https://user-images.githubusercontent.com/53509789/210571932-489658be-368c-445f-a72f-223a3e577e6a.png">

## Performance
### Cell prefetching
<img width="784" alt="스크린샷 2023-01-04 오후 10 54 15" src="https://user-images.githubusercontent.com/53509789/210571933-74674ca5-de92-445f-b26d-fb315a25e4ba.png">
<img width="1003" alt="스크린샷 2023-01-04 오후 10 54 35" src="https://user-images.githubusercontent.com/53509789/210571938-555739f6-0744-4ecb-925c-0f4b534bcb6d.png">
<img width="1183" alt="스크린샷 2023-01-04 오후 10 55 05" src="https://user-images.githubusercontent.com/53509789/210571939-de1bb33e-c67f-4616-9f6b-ee84985b8286.png">
- 스크롤을 내리며 셀의 콘텐츠 준비 과정이 두배 정도 빨라졌다.
- Main Queue에서 큰 이미지를 디코딩하며 스크롤이 일시적으로 중단되는 현상이 있었으며 이는 iOS15에서 이미지를 준비하는 새로운 기능이 추가되어 해결이 가능하다. 
이를 통해 코드로 해당 프로세스를 제어할 수 있으며 이는 비동기적으로 사용할 수 있기 때문에 이미지가 디코딩 되는 동안 메인 큐에서 이벤트의 자유로운 처리가 가능해진다.
- 이미지 크기를 효율적으로 조정하여 메모리를 절약할 수 있어졌다. 

## Security and Privacy
<img width="1453" alt="스크린샷 2023-01-04 오후 10 56 39" src="https://user-images.githubusercontent.com/53509789/210571941-97db6b5e-eaa5-4acd-9f6a-62c24a7e178a.png">
- 현 위치에 대한 일회성 접근 권한 허용 버튼 API 도입
<img width="1203" alt="스크린샷 2023-01-04 오후 10 35 44" src="https://user-images.githubusercontent.com/53509789/210792447-078b4075-6ccc-4744-9f78-ed6129c16057.png">
- 붙여넣기 배너
 - (의도적으로) 직접 복사하여 붙여넣기 시, 배너를 띄우지 않는다.
