
# Topic

## Families
WidgetKit이 제공하는 세가지 패밀리가 있다.

<img width="1520" alt="스크린샷 2023-03-25 오후 12 55 38" src="https://user-images.githubusercontent.com/53509789/227691392-d322737f-f617-419e-ab31-3eafe4b57a00.png">

- SysmtemSmall
- SysmtemMedium
- SysmtemLarge


## Timelines 

Timeline Provider는 위젯의 엔진이다.  
provider에서 하나의 entry or 여러개의 entry를 return 할 수 있다.  

그렇다면 타임라인의 마지막은 어떻게 될까?, 더 많은 entry을 제공하기 위해서는 어떻게 해야할까?

<img width="1493" alt="스크린샷 2023-03-25 오후 12 57 58" src="https://user-images.githubusercontent.com/53509789/227692003-61b98030-1373-43ec-a23e-9ccaa3b349a0.png">

TimelineReloadPolicy을 이용해야한다.

<img width="400" alt="스크린샷 2023-03-25 오후 1 00 52" src="https://user-images.githubusercontent.com/53509789/227692654-ffc5b7b8-815d-42d4-8515-226c35a13589.png">

- atEnd는 타임라인의 마지막 항목이 표시될 때, 업데이트 되도록 WidgetKit에 지시한다.
- after는 지정한 날짜에 업데이트 되도록 WidgetKit에 지시한다.
- never는 위젯을 독립적으로 업데이트 하지 않음을 지시한다.

최상의 사용자 경험을 제공하고, 사용자가 위젯을 볼 수 있는 시간에 최대한 가깝게 위젯 업데이트를 제공하기 위해
우리는 TimelineReloadPolicy를 이용하여 지능적으로 업데이트를 예약해야한다.


## Configuration
사용자는 홈 화면에서 바로 위젯을 커스텀 할 수 있다.
WidgetKit은 SiriKit에 의해 구동이 되며,
커스텀을 위한 핵심 기술은 INIntent, 사용자 지정 의도이다.
(코드에서 자세히)


## Deep linking
위젯은 애니메이션이나 사용자 interaction 이 없다.
하지만, 위젯에서 앱으로 딥 링크를 연결할 수 있다.
- SysmtemSmall은 하나의 큰 탭 영역을 가지고 있다.
- SysmtemMedium 및 SysmtemLarge는 새로운 SwiftUI link API를 이용하여 위젯 내에서 탭 가능한 영역을 생성할 수 있다.


# Code-along

위의 네가지 주제에 따라, 코드를 확인해본다.

## Family
이전 part1 세션에서는 Small 사이즈만을 지원하는 위젯을 보았다.

<img width="604" alt="스크린샷 2023-03-25 오후 1 18 47" src="https://user-images.githubusercontent.com/53509789/227695649-7f902952-1e54-401a-9bec-e026845e1115.png">

SupportedFamiles에 SystemMedium을 추가하여 두 가지 사이즈를 지원하는 위젯을 만든다.

<img width="510" alt="스크린샷 2023-03-25 오후 1 19 15" src="https://user-images.githubusercontent.com/53509789/227695647-4a2bd3af-d450-4871-b9cf-f40cbefebd5b.png">

WidgetKit이 제공하는 environment value인 Widget Family를 이용하여 사이즈에 맞는 뷰를 보여준다.


## Timeline
타임라인을 적용해보자.
해당 프로젝트에서는 캐릭터가 언제 치유되는지 알 수 있고, 그 시간을 이용하여 치유될때까지의 전체 타임라인을 생성해본다.

<img width="585" alt="스크린샷 2023-03-25 오후 1 26 24" src="https://user-images.githubusercontent.com/53509789/227695928-aaa5e267-40be-4366-8bee-d59830c523e0.png">

endTime을 치유가 완료될때의 시간으로 설정하고 endTime 전 까지 1분 주기로 entry를 추가한다.  
이제 WidgetKit은 해당 타임라인이 소진될 때까지 다시 로드하려하지 않는다.

추가적으로, 위젯이 스택 안에서 사용된다면 시스템은 지능적으로 위젯들을 회전 시키는데  
이때 위젯은 relevance를 이용하여 시스템에게 우선순위를 알려줄 수 있다.
해당 프로젝트에서는 healthLevel을 이용하여 사용하였다.


## Configuration
지금까지 위젯의 캐릭터를 panda로 지정해서 만들었다.
사용자가 해당 캐릭터를 변경하고 구성하게 변경해보자.

<img width="509" alt="스크린샷 2023-03-25 오후 12 08 18" src="https://user-images.githubusercontent.com/53509789/227696259-2095e981-1b5b-4927-8fb8-c563fe0c547c.png"> <img width="505" alt="스크린샷 2023-03-25 오후 12 08 51" src="https://user-images.githubusercontent.com/53509789/227696261-2e2cfdb3-2888-44fe-bc12-f68ac15beeb1.png">

이를 위해, 위와 같이 siri intent 파일을 추가한다.  
해당 파일에는 다음과 같이 Category, Title, Description을 설정할 수 있다.  

<img width="600" alt="스크린샷 2023-03-25 오후 12 11 01" src="https://user-images.githubusercontent.com/53509789/227696262-e130ef51-182a-4ccc-a81f-f281565f6555.png">

또한, 파라미터로 우리의 캐릭터 Hero를 설정하고 해당 열거형 타입(Hero)을 Enum Editor에서 편집할 수 있다.

<img width="600" alt="스크린샷 2023-03-25 오후 12 11 07" src="https://user-images.githubusercontent.com/53509789/227696264-e951ac58-d89b-4dc4-b3e4-0b2e6413f9a1.png">
<img width="500" alt="스크린샷 2023-03-25 오후 12 12 27" src="https://user-images.githubusercontent.com/53509789/227696265-a937cb5a-176e-4d70-9732-906862639ffe.png"> <img width="500" alt="스크린샷 2023-03-25 오후 12 13 03" src="https://user-images.githubusercontent.com/53509789/227696266-c9753bc2-4a18-4e31-94bd-6e53df69da1f.png">

이 intent를 이용하기 위해서는, 

1. 위젯 Static Configuration에서 Intent Configuration으로 변경

<img width="631" alt="스크린샷 2023-03-25 오후 12 13 45" src="https://user-images.githubusercontent.com/53509789/227696267-e4e01821-0dc7-4b79-b99e-108a7293a9c6.png"> <img width="962" alt="스크린샷 2023-03-25 오후 2 48 57" src="https://user-images.githubusercontent.com/53509789/227699178-b1dba6f1-230b-48a6-a804-9f05cb586228.png">


2. TimelineProvider -> Intent TimelineProvider 변경
3. Snapshot, Timeline 변경  

<img width="641" alt="스크린샷 2023-03-25 오후 12 19 38" src="https://user-images.githubusercontent.com/53509789/227696268-ad3ca03c-e8c7-4a2d-8b37-78aa73e59fdf.png">

4. 선택한 캐릭터로 구성하기 위해, 다음과 같은 character 메서드를 만들어주고, timeline의 selectedCharacter를 다음과 같이 바꿔준다.

<img width="624" alt="스크린샷 2023-03-25 오후 12 27 20" src="https://user-images.githubusercontent.com/53509789/227696270-3378edbd-4db8-4999-99f9-7136d8cf2027.png">

빌드해보면, 다음과 같이 default인 팬더가 보이고  

<img width="311" alt="스크린샷 2023-03-25 오후 12 28 02" src="https://user-images.githubusercontent.com/53509789/227696271-4ba7566e-4c1f-4220-8a54-49e94132608f.png">

Edit을 이용하여 다른 캐릭터로 변경이 가능한 것을 확인 할 수 있다!

<img width="283" alt="스크린샷 2023-03-25 오후 12 28 10" src="https://user-images.githubusercontent.com/53509789/227696272-4a5e076e-95f9-4737-acdc-cf9ae7cd87ff.png"> <img width="355" alt="스크린샷 2023-03-25 오후 12 28 20" src="https://user-images.githubusercontent.com/53509789/227696273-d9615afe-efc0-4207-b18a-382e13172f78.png"> <img width="241" alt="스크린샷 2023-03-25 오후 12 28 28" src="https://user-images.githubusercontent.com/53509789/227696274-dcd3006b-6b04-4476-a0ef-b4b14912522a.png">



## Deep linking
위젯에서 앱으로의 연결을 하기 위해서는 widgerURL을 추가해주면 된다. 

![Simulator Screen Shot - iPhone 14 Pro - 2023-03-25 at 14 40 33](https://user-images.githubusercontent.com/53509789/227699063-9ea94cd2-440b-4298-af2d-81ae006982ea.png)
![Simulator Screen Shot - iPhone 14 Pro - 2023-03-25 at 14 40 25](https://user-images.githubusercontent.com/53509789/227699060-7933668a-4b6d-48e7-95cb-bfddab507fbd.png)
![Simulator Screen Shot - iPhone 14 Pro - 2023-03-25 at 14 47 07](https://user-images.githubusercontent.com/53509789/227699134-2da4ff90-87f0-49d4-a17c-ace881fafcb4.png)
