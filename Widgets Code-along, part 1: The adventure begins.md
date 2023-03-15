해당 세션은 제목에서 나와있듯, 코드와 함께 진행하는 세션입니다.

## What is a widget?
<img width="338" alt="스크린샷 2023-03-15 오후 7 17 38" src="https://user-images.githubusercontent.com/53509789/225290144-08d80ac2-0a9d-421e-be39-135089297704.png">  

위젯은 시간에 따라 업데이트 되는 SwiftUI의 View일뿐이다.

## Code
세션에서 제공해준 프로젝트를 빌드해보면 다음과 같은 앱을 볼 수 있다.

<img width="250" alt="스크린샷 2023-03-15 오후 7 17 38" src="https://user-images.githubusercontent.com/53509789/225293284-ef1d7bb3-f42a-4125-a435-1ad4d2ce733d.png"> <img width="250" alt="스크린샷 2023-03-15 오후 7 17 38" src="https://user-images.githubusercontent.com/53509789/225293297-de95bd30-47ab-49c0-8c79-5bfe5b0c249e.png"> 

해당 프로젝트에 위젯을 추가해보자.

1. 위젯을 만들기 위해 Widget Target을 활성화 해야한다.

<img width="300" alt="스크린샷 2023-03-15 오후 7 17 58" src="https://user-images.githubusercontent.com/53509789/225290148-83a474d7-3135-48f1-9bda-202bd03c5187.png"> <img width="300" alt="스크린샷 2023-03-15 오후 7 14 29" src="https://user-images.githubusercontent.com/53509789/225290133-87a29220-be94-407b-a44f-834919531083.png">

<img width="208" alt="스크린샷 2023-03-15 오후 7 19 21" src="https://user-images.githubusercontent.com/53509789/225290150-6f7484b4-3df8-4662-af1c-1c901433286b.png"> <img width="304" alt="스크린샷 2023-03-15 오후 7 20 21" src="https://user-images.githubusercontent.com/53509789/225290154-e66ee573-cf89-4ec8-9c0e-7d47d4858547.png">  
활성화를 하면 다음과 같이 파일들이 생성되는 것을 확인할 수 있다.

2. 위젯에 사용할 파일들을 Widget Target에 추가시킨다.

<img width="1512" alt="스크린샷 2023-03-15 오후 7 48 07" src="https://user-images.githubusercontent.com/53509789/225290173-137a0de8-18cb-40fe-87ee-7a78d86cc492.png">

3. 위젯 수정

<img width="810" alt="스크린샷 2023-03-15 오후 7 23 28" src="https://user-images.githubusercontent.com/53509789/225290156-77e87036-f71b-43a4-a7b8-16878d309963.png">

먼저, Preview에 AvatarView를 넣어보면 다음과 같이 확인이 된다.
<img width="838" alt="스크린샷 2023-03-15 오후 7 23 52" src="https://user-images.githubusercontent.com/53509789/225290161-28e9c378-9602-4b24-a52a-3ba8debc9b3e.png">  

body를 AvatarView를 넣어보자.
AvatarView를 초기화할때, 캐릭터가 들어간다. 또한, 해당 데이터는 Entry에서 가져와야하기 때문에 우리는 Entry에 캐릭터 프로퍼티를 추가해야한다.

<img width="566" alt="스크린샷 2023-03-15 오후 8 32 12" src="https://user-images.githubusercontent.com/53509789/225297595-2ce919c0-d9d3-4bf8-bb53-a8493cd5419a.png">

위젯에서 Entry는 위젯의 핵심 엔진인 Timeline Provider에서 가져온다.

Timeline Provider는 위젯에서
- 하나의 Entry만 보여줄때는 Snapshot을 제공한다.
- 여러 개의 Entry를 보여줄때는 Full Timeline을 제공한다.

해당 프로젝트는 Full Timeline이 필요하지 않으므로, 다음과 같이 하나의 캐릭터 데이터를 전달하면 된다.

<img width="689" alt="스크린샷 2023-03-15 오후 8 43 42" src="https://user-images.githubusercontent.com/53509789/225300178-39d42a98-f641-402b-9896-981abbd5f471.png">


4. 지원 사이즈 설정

<img width="838" alt="스크린샷 2023-03-15 오후 7 23 52" src="https://user-images.githubusercontent.com/53509789/225290161-28e9c378-9602-4b24-a52a-3ba8debc9b3e.png">  
<img width="812" alt="스크린샷 2023-03-15 오후 7 24 11" src="https://user-images.githubusercontent.com/53509789/225290167-95d98a4d-8ec7-4d14-a5b5-a7a94184a26f.png">
<img width="813" alt="스크린샷 2023-03-15 오후 7 24 30" src="https://user-images.githubusercontent.com/53509789/225290171-3e995e08-1c82-4aee-8e4a-de6a6c8eb6e3.png">

아무것도 설정하지 않는다면, 위과 같이 사용자가 모든 사이즈를 설정할 수 있다.  
하지만 이는 공간을 잘 활용하지 못한(지원할 준비가 안되어있는 뷰) 이므로, 아래과 같이 body에 지원할 사이즈만 설정할 수 있다.  

<img width="664" alt="스크린샷 2023-03-15 오후 8 46 16" src="https://user-images.githubusercontent.com/53509789/225300182-3215172e-b839-44d9-bd82-0ccc170fbfb2.png">

위와 같이 설정한다면, 단일 크기 지원 위젯을 확인할 수 있다.  

<img width="312" alt="스크린샷 2023-03-15 오후 8 51 30" src="https://user-images.githubusercontent.com/53509789/225301162-81e22892-6bf8-4332-86a8-b654d8605811.png">
