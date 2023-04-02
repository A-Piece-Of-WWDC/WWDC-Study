# Widgets Code-along

이전 세션에 이어, 해당 세션에서는 리더 보드라는 새로운 위젯이 추가됐다.

![Simulator Screen Shot - iPhone 14 Pro - 2023-03-25 at 14 40 33](https://user-images.githubusercontent.com/53509789/227699063-9ea94cd2-440b-4298-af2d-81ae006982ea.png) <img width="327" alt="스크린샷 2023-04-02 오후 3 33 47" src="https://user-images.githubusercontent.com/53509789/229338880-830f12e9-d322-4349-b24f-1fffc1fb4629.png">

# Topic

해당 세션에서는 다음 네가지 topic에 대해 이야기를 한다.

- URL Session
- Using Link
- Widget bundles
- Dynamic Configuration 


## URL Session
위젯 또한 네트워크 요청을 통해 데이터를 얻을 수 있다.

<img width="1109" alt="스크린샷 2023-04-02 오후 4 47 16" src="https://user-images.githubusercontent.com/53509789/229339742-0b6bc53e-5489-4c07-a506-35ca797459c3.png">
<img width="934" alt="스크린샷 2023-04-02 오후 4 47 27" src="https://user-images.githubusercontent.com/53509789/229339747-698f1fdc-e7ca-4527-8389-b9f24cd43f66.png">

- timeline provider api는 비동기 작업을 쉽게 수행할 수 있도록 completion hanlder로 구축이되어있다.
- 따라서 다음과 같이 `loadLeaderboardData`와 같은 일반적인 url session api을 호출하여 데이터를 불러온다.

- 위젯은 background session을 포함한 모든 URL 세션에 응답한다. 하지만 위젯은 앱 delegate가 존재하지 않는다. 

**그렇다면 위젯은 무엇을 해야하는지 어떻게 알 수 있을까?**

<img width="770" alt="스크린샷 2023-04-02 오후 4 57 31" src="https://user-images.githubusercontent.com/53509789/229340128-bac069ec-8da7-4dab-a7cf-3df8219130bf.png">

- 위젯 configuration에는 `onbackgroundURLSessionEvents` 이라는  modifier가 존재하며, 이 modifier가 delegate 메서드와 유사한 역할을 한다.

## Using Link
- 캐릭터를 탭하여 앱의 특정 세부화면을 실행하기 위해, 이전 세션에서는 `widgetURL` 메서드를 이용했었다. 이번 세션에서는, SwiftUI Link를 이용해보자.

<img width="918" alt="스크린샷 2023-04-02 오후 3 37 29" src="https://user-images.githubusercontent.com/53509789/229338889-17946073-988f-48f9-90fb-2aab76c7b5b9.png">

다음과 같이, 리더보드의 View의 각 캐릭터 라인을 만드는 HStack을 Link로 감싸면 된다.


## Widget bundles

위젯 갤러리를 보면, 이전에 만들었던 위젯들은 보이지 않는 것을 볼 수 있다.

![Simulator Screen Shot - iPhone 14 Pro - 2023-04-02 at 15 38 31](https://user-images.githubusercontent.com/53509789/229339497-65cf4129-59ca-4c5d-91a2-217c23409051.png)
 
프로세스당 하나의 메인을 가지는데, 여기서 @main을 리더보드로 옮겼기 때문.
<img width="763" alt="스크린샷 2023-04-02 오후 5 23 40" src="https://user-images.githubusercontent.com/53509789/229341247-57665227-b149-4fc9-b9ac-4644082bf630.png">

따라서 여러 개의 위젯을 추가하기 위해서는, 다음과 같이 위젯 번들을 만들고 번들로 @main 어트리뷰트를 변경해야한다. 

<img width="551" alt="스크린샷 2023-04-02 오후 3 41 10" src="https://user-images.githubusercontent.com/53509789/229338903-4a30ba8e-2f5c-4d28-b1d1-d0fde24ab048.png">

번들을 이용한다면, 여러 위젯들을 확인할 수 있다.

![Simulator Screen Shot - iPhone 14 Pro - 2023-04-02 at 15 41 40](https://user-images.githubusercontent.com/53509789/229339525-caefa06c-68a0-4981-ab0b-bbc046b1ea8e.png) ![Simulator Screen Shot - iPhone 14 Pro - 2023-04-02 at 15 41 43](https://user-images.githubusercontent.com/53509789/229339526-6d1ee1da-5cee-4199-8ba3-b75937760a78.png)


## Dynamic Configuration
이전 세션에서는 하드 코딩을 통해 위젯의 configuration을 설정하였다.  
하지만, 이러한 옵션들을 사전에 알지 못하여 하드코딩을 할 수 없다면?

<img width="517" alt="스크린샷 2023-04-02 오후 3 47 31" src="https://user-images.githubusercontent.com/53509789/229338908-ccac5610-1249-4486-95f6-8cfe6f37bab4.png">
<img width="709" alt="스크린샷 2023-04-02 오후 3 47 53" src="https://user-images.githubusercontent.com/53509789/229338911-843d1ff6-5887-4e9e-9a11-c5ce58341098.png">

configuration은 SiriKit Intent이기 때문에 다른 Intent 기반 기능과 동일하게 Extension을 사용하여 옵션의 동적 목록을 제공할 수 있다.  
즉, 하드코딩이 필요없다.

<img width="898" alt="스크린샷 2023-04-02 오후 3 49 55" src="https://user-images.githubusercontent.com/53509789/229338912-bb9cc736-531b-4dfb-ada4-9651dfb3d7c8.png">

이것은 이전 세션의 static Intent와 비슷해보이지만, Enum 타입이 아닌 커스텀 타입이다.

<img width="918" alt="스크린샷 2023-04-02 오후 4 10 24" src="https://user-images.githubusercontent.com/53509789/229338913-405f9195-2da1-45cb-9352-d943a338cf92.png">

해당 타입의 값들은 Intent Handler에서 비동기로 가져온다.

이제 기존 Intent가 아닌, Dynamic Intent를 통해 위젯을 업데이트 해보자.


<img width="910" alt="스크린샷 2023-04-02 오후 4 14 12" src="https://user-images.githubusercontent.com/53509789/229338916-8a2e7d3f-9db9-4d42-9499-f350d3af3df8.png">

열거형 값이 아니기 때문에 더 이상 `character` 메서드는 필요하지 않다.

<img width="897" alt="스크린샷 2023-04-02 오후 4 16 08" src="https://user-images.githubusercontent.com/53509789/229338917-bfa6fd25-94f2-449a-b457-2571427780da.png">

해당 메서드 대신, 커스텀 타입의 identifier를 통해 Selected Character 정보를 가져올 수 있다.

Dynamic Intent를 설정하면  
다음과 같이, Intent Handler에서 가져온 여러 캐릭터들을 확인하고 설정할 수 있다.

![Simulator Screen Shot - iPhone 14 Pro - 2023-04-02 at 16 23 23](https://user-images.githubusercontent.com/53509789/229339532-ec7ba708-0111-4dbb-8278-b03abd968388.png) ![Simulator Screen Shot - iPhone 14 Pro - 2023-04-02 at 16 23 20](https://user-images.githubusercontent.com/53509789/229339531-13c880f8-1adb-4c07-a112-ae5c6fed3ab7.png) ![Simulator Screen Shot - iPhone 14 Pro - 2023-04-02 at 16 23 29](https://user-images.githubusercontent.com/53509789/229340934-3a3b8182-b19b-4268-9b36-0c66261be510.png)

