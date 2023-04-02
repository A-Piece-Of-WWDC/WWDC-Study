# Meet WeatherKit

- 기상데이터는 우리의 생활과 밀접한 관련이 있기에 올바른 데이터를 얻는 것은 매우 중요함
- 이런 이유로 Apple에서 새롭게 만든 Framework에 대한 세션
    
    ![스크린샷 2023-03-25 오후 12 08 53](https://user-images.githubusercontent.com/87598209/229332620-d5305d29-044c-4735-a973-bce42c76ef1f.png)

- WeatherKit은 세계적 수준의 기상 서비스인 Apple Weather Service를 기반으로 작동됨
- 고해상도 날씨 모델을 사용함
- 머신 러닝과 예측 알고리즘을 사용함
- 특정 지역의 기상 정보를 전 세계에 보여줄 수 있음

![스크린샷 2023-03-25 오후 12 12 00](https://user-images.githubusercontent.com/87598209/229332622-70d8499e-2aef-4943-83fd-dabdc6fcee4e.png)

- WeatherKit에서는 위에서 보여지는 모든 정보를 얻을 수 있음

## Privacy

- WeatherKit은 사용자의 위치 정보를 필요로 하는 프레임워크이다. 따라서 사용자의 개인 정보가 잘못 이용될 소지가 있어서 사용자의 정확한 위치에 대한 기상 예보를 제공하는 것이 아닌 인근 지역의 기상 예보를 전달함
    
    ![스크린샷 2023-03-25 오후 12 16 25](https://user-images.githubusercontent.com/87598209/229332624-53d15ecc-b588-4d50-9d23-caea6506ffd4.png)

- 위치는 기상 예보를 제공하는데에만 사용함
- 절대 공유되거나 따로 판매하지 않음
- WeatherKit은 사용자의 개인 정보 보호를 보다 쉽게 만들었음

## Available Weather Datasets

### Current Weather

![스크린샷 2023-03-25 오후 12 18 36](https://user-images.githubusercontent.com/87598209/229332630-648b8e01-949a-45d6-90c4-0bcea98ef393.png)

- 위와 같은 정보들을 Current Weather을 통해 얻을 수 있음.
- 자외선 지수, 기온, 풍속, 습도 등등

### Minute forecast

![스크린샷 2023-03-25 오후 12 19 48](https://user-images.githubusercontent.com/87598209/229332634-07cd964c-7aef-4f6e-b372-88b3817e28cb.png)

- 분 단위 예보는 가능한 지역이 있고 불가능한 지역이 있음
- 가능한 지역의 경우 다음 1시간 동안 1분 마다의 강수 상태를 알려줌
- 해당 데이터는 우산을 챙겨야할지 말지를 결정하는데에 유용하게 사용할 수 있음

### Hourly forecast

![스크린샷 2023-03-25 오후 12 21 54](https://user-images.githubusercontent.com/87598209/229332638-fafc5d80-5fdb-4f96-ac63-edf5bec16796.png)

- 최대 240시간에 대한 데이터를 제공
- 각각의 시간마다 우리는 위의 데이터들을 알 수 있음
- 습도, 기압, 이슬점 등등

### Daily forecast

![스크린샷 2023-03-25 오후 12 23 29](https://user-images.githubusercontent.com/87598209/229332639-2959a20e-36df-40ab-8d8c-a2728fe038e3.png)

- 10일에 대한 데이터들을 가지고 있음
- 하루 전체의 정보를 제공
- 최저, 최고 기온, 일몰, 일출 시간 등등

### Weather alerts

![스크린샷 2023-03-25 오후 12 24 54](https://user-images.githubusercontent.com/87598209/229332641-48a14fc4-aa37-41b5-ac82-7813f28a6653.png)

- 해당 데이터에는 사용자들의 안전과 관련된 날씨 정보를 제공해줌

### Historical weather

![스크린샷 2023-03-25 오후 12 25 38](https://user-images.githubusercontent.com/87598209/229332644-681e7b5c-d042-4b57-a29a-3ed74a1da6db.png)

- 과거의 날씨 정보를 이용해서 기상의 동향을 파악할 때 사용할 수 있는 데이터임
- 시작과 종료 일자를 지정하는 방식으로 이용

## Requesting weather

- WeatherKit을 이용하기 위해서는 2가지 방법이 있음
    - WeatherKit 네이티브 프레임워크를 이용하는 방식과 WeatherKit REST API를 방식
        
        ![스크린샷 2023-03-25 오후 1 46 00](https://user-images.githubusercontent.com/87598209/229332647-bfc830f6-98c3-4fae-87b4-ac9e84eb8e87.png)


### Native Framework

- 생각보다 너무 간단하게 가능
    
    ![스크린샷 2023-03-25 오후 1 35 25](https://user-images.githubusercontent.com/87598209/229332648-879402ca-bf38-4817-baad-82032a32d3a4.png)

    - 먼저 Developer 페이지에서 App Service에 WeatherKit을 등록한다
        
        ![스크린샷 2023-03-25 오후 1 36 24](https://user-images.githubusercontent.com/87598209/229332653-4f0625ea-fe2a-4e94-86ca-5aa41c93b1a8.png)

    - 그 다음 WeatherKit을 사용하려는 프로젝트의 Xcode Capabilities에서 WeatherKit을 추가해주면 끝 !

![스크린샷 2023-03-25 오후 1 25 51](https://user-images.githubusercontent.com/87598209/229332658-db1305ff-1d99-4934-9513-9265251c3b70.png)

- 다음으로 코드를 작성해준다! weather 객체를 얻기까지 단 3줄이면 됨

### 주의 사항

- 단, WeatherKit을 통해 날씨 정보를 받을 때에는 Apple Weather Service 데이터 출처 URL과 로고를 꼭 명시해줘야함
    
    ![스크린샷 2023-03-25 오후 1 41 37](https://user-images.githubusercontent.com/87598209/229332663-61b1ed9a-0bc8-4001-8094-1fc3dad98d0f.png)

- 위 코드 처럼 attribution을 받은 다음에 링크와 로고를 뷰에 표시해줘야함
- attributedURL에는 데이터에 대한 저작권 정보가 담겨 있음
- 링크는 SFSafariViewController를 통해 열림
    
    ![스크린샷 2023-03-25 오후 1 44 43](https://user-images.githubusercontent.com/87598209/229332668-a0f8df6a-bbd6-4448-937b-7fb3ae5aa9f5.png)

    - 위의 스크린샷 하단을 보면 Appler Weather Service의 로고와 링크가 표시된 걸 확인할 수 있음

### WeatherKit REST API

- WeatherKit 데이터를 REST API를 통해서도 제공받을 수 있음.
- 즉, 다른 어느 플랫폼에서든 Apple Weather Service를 이용할 수 있음

![스크린샷 2023-03-25 오후 1 47 37](https://user-images.githubusercontent.com/87598209/229332671-95df6611-b2ec-4e54-9fbf-77dd9b658a1e.png)

- 예시 코드
- JWT를 통해 인증함
- 파라미터 중에서 country값은 기상 경보 데이터를 받을 때만 필요로함

## Publishing Requirements

- 앱스토어에 퍼블리싱하거나 REST API를 이용해서 다른 플랫폼에서 출시를 하기 위해서는 몇 가지 요구사항이 있음
    
    ![스크린샷 2023-03-25 오후 1 53 37](https://user-images.githubusercontent.com/87598209/229332673-4eb14249-c405-4156-9ea7-ed917981d04c.png)

- 이 요구사항들은 네이티브 프레임웍을 쓰든 REST AP를 쓰든 모두 똑같이 적용되는 사항임
- 아까 위의 예시에서 본 것처럼 반드시 링크와 로고를 표시해줘야함
- 만약 기상 경보 서비스를 쓸 경우 관련된 링크도 제공해줘야함

- 샘플 앱이 있다고 하니까 좀 더 자세히 공부해보려면 고거 확인해보는 것도 좋을 듯
