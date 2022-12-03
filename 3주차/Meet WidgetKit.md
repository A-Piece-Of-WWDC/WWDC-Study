# Meet WidgetKit

### WidgetKit이란?

- 우리의 앱이 가지고 있는 콘텐츠들을 적절하게, 한 눈에 알아보기 쉽게 위젯으로 보여줄 수 있도록 기능을 제공해주는 프레임워크
- 사용자에게 간편한 경험을 주고 개발자에게도 다양한 플랫폼에 손쉽게 적용할 수 있어야하므로 위젯의 UI와 WidgetKit은 모두 **SwiftUI**로 만들어졌다

- 보통 사람들은 하루에 90번 이상 홈화면으로 이동한다고 한다
- 그리고 아주 잠깐의 시간만 홈화면에서 보낸다

### 무엇이 좋은 위젯 경험을 만들어 줄까

- 3가지 주요 goal이 존재
    - Glanceable
    - Relevant
    - Personalization
    

### Glanceable

- 가장 작은 위젯의 경우 가지고 있는 공간이 매우매우 한정적이다
    
    ![스크린샷 2022-11-30 오전 12 00 59](https://user-images.githubusercontent.com/87598209/205020794-134ba616-2919-470f-9f27-3bd0aa0e8161.png)

- 유저들은 홈스크린에서 아주 적은 시간을 유지함. 따라서 우리는 한정된 시간동안 최대한의 value를 유저에게 줄 수 있어야한다
- 간단하고 복잡하지 않아야함. 그 어떤 버튼도 존재해선 안된다
- 콘텐츠에 집중해야함(위젯은 미니앱이 아님)

### Relevant

- 위젯을 사용하는 것은 반드시 유의미 해야한다. 유저가 위젯을 사용함으로써 얻는 이점이 존재해야함

### Personalization

- 좋은 위젯은 사용자가 위젯을 개인화할 수 있어야한다
    
    ![스크린샷 2022-11-30 오전 12 12 33](https://user-images.githubusercontent.com/87598209/205020879-bebe2cd2-3b4a-413a-973b-db711bfaa31b.png)

- 개인 취향을 반영할 수 있게 다양한 위젯 사이즈를 지원해주면 좋다
- 유저가 위젯의 configuration을 자유롭게 변환시킬 수 있게 만들어주자
    - 모든 configuration 옵션들은 Intent를 활용하여 만들어줄 수 있음
        
        ![스크린샷 2022-11-30 오전 12 14 01](https://user-images.githubusercontent.com/87598209/205021016-155df430-4032-49fd-be73-bd1c6981909b.png)


## How WidgetKit works

- 위젯은 항상 홈스크린 위에 떠있다. 그렇다면 앱과의 싱크는 언제 어느시점에 맞춰지는걸까?
    - WidgetKit extension
        
        ![스크린샷 2022-11-30 오전 12 41 05](https://user-images.githubusercontent.com/87598209/205021036-af7b03d1-7d37-4938-8135-012487685c4d.png)

        ![스크린샷 2022-11-30 오전 12 42 10](https://user-images.githubusercontent.com/87598209/205021049-3df48298-5b43-4566-890a-a9e453d5ca82.png)

- 캘린더가 업데이트 되면 앱은 timeline을 reload시켜주는 API를 호출한다.
- 이 때, WidgetKit extension은 새로운 timeline을 받아서 widget을 업데이트 시켜준다.

## Widget 정의하기(Defining a Widget)

- 위젯은 다음과 같은 특성들을 가지고 있음
    - kind
    - configuration
    - supportedFamilies
    - placeholder
    

### Kind

- 단일 extension 하나로 우리는 여러 종류의 위젯을 만들고 업데이트 시켜줄 수 있다.
    
    ![스크린샷 2022-11-30 오전 12 50 32](https://user-images.githubusercontent.com/87598209/205021066-582ad68e-548d-4791-8bfa-2c7954454c8f.png)


### Configuration

- Widget에는 두 가지 configuration 타입이 존재한다
    
    ![스크린샷 2022-11-30 오전 12 53 19](https://user-images.githubusercontent.com/87598209/205021086-7c22f62a-70e3-4ab0-9099-6a564e414950.png)

    - StaticConfiguartion
        
        ![스크린샷 2022-11-30 오전 12 53 59](https://user-images.githubusercontent.com/87598209/205021113-a8eefde7-41d4-437e-aec5-d82a7d765463.png)

        - 위처럼 health앱의 경우 사용자가 따로 설정을 바꾸거나 그럴 필요가 없다
        - 그냥 단지 너의 활동상태야 이렇게 보여주는게 다인 위젯임
    - IntentConfiguartion
        
        ![스크린샷 2022-11-30 오전 12 56 46](https://user-images.githubusercontent.com/87598209/205021136-97239042-ce0e-424b-8756-3939319fbf8f.png)

        - 위와 같은 Reminder 위젯의 경우 사용자가 계속해서 리스트를 바꿔주는 등의 설정이 필요하다
        - 또 다른 예로, 주식 위젯같은 경우 내가 보고싶은 주식이 있을 수도 있다. 보고 싶은 주식을 설정할 수 있어야한다

### supportedFamilies

- 위젯은 다양한 크기를 지원한다

![스크린샷 2022-11-30 오전 12 59 04](https://user-images.githubusercontent.com/87598209/205021155-5d2c1be8-3f89-4a10-918a-d652945e1f01.png)

### placeholder UI

- 위젯은 placeholder UI를 제공한다
- 플레이스홀더에 유저데이터를 사용해서는 안됨

![스크린샷 2022-11-30 오전 1 02 15](https://user-images.githubusercontent.com/87598209/205021186-b9b293c5-104a-48b7-b062-2e79a05537a0.png)

## 샘플 위젯 코드

![스크린샷 2022-11-30 오전 1 04 21](https://user-images.githubusercontent.com/87598209/205021203-ed6964e3-b8b7-4b65-aa0b-f7f45d4b4cc8.png)

## TimelineProvider

- TimelineProvider는 위젯의 UI들을 언제 업데이트시켜줄지를 알려주는 타입
- TimelineEntry, snapShot메서드와 timeline 메서드로 이루어져 있다

![스크린샷 2022-12-01 오후 6 10 17](https://user-images.githubusercontent.com/87598209/205021399-f0757337-1197-4668-9a88-df2443f190d8.png)

- Snapshot
    - Snapshot은 유저가 위젯갤러리에서 보게될 위젯의 모습
    - 따라서 단 하나의 entry만을 가질 수 있다
    
    ![스크린샷 2022-12-01 오후 6 09 36](https://user-images.githubusercontent.com/87598209/205021225-a544ba0a-2f0a-4e93-a89a-a493aa6c4c01.png)

- Timeline
    - Snapshot이 단 하나의 entry를 리턴했다면, Timeline 여러 개의 entry를 리턴할 수 있다
    - 위젯이 업데이트되어야하는 시간대별로 Widget을 업데이트 시켜주는 메서드
        
        ![스크린샷 2022-12-01 오후 6 22 45](https://user-images.githubusercontent.com/87598209/205021328-e01f0051-3b44-4344-ac84-e0e4c1b6aa07.png)

    - Policy는 reload 정책을 의미함
        - 구체적으로 어느 시점에 배열에 있는 엔트리들로 UI를 업데이트 시켜줄지 의미
            - atEnd
                - 우리가 제공한 타임라인의 마지막 순간에 리로드 시켜줄지
            - after(date: Date)
                - 특정 시간대에 리로드 시켜줄지
            - never
                - 절대 리로드 시켜주지 않을지
- 하지만 이렇게 시간대를 정해주고 리로드 시켜주는 건 조금 비효율적이고 관리가 어려울 수 있다
- 특정한 액션이 있을때만 UI를 업데이트 시켜주는 것도 가능하다
![스크린샷 2022-12-01 오후 6 29 47](https://user-images.githubusercontent.com/87598209/205021446-4844c37d-b9ff-4d08-87fb-04c820bcb4c1.png)

