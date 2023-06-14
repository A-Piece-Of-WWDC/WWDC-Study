# What's new in App Store Connect
앱 스토어 커넥트에 어떤 신규 기능이 생겼는지 알려주는 세션

# Recent Updates

- TestFilght로 테스트 그룹 관리가 쉬워짐. 한번의 클릭만으로 신속하게 versions나 build groups 탭에서 직접 빌드에 테스터 그룹을 추가하거나 삭제할 수 있음
- Apple Wallet을 사용하는 앱으로 전환할 수 있도록 업데이트
- 제출 과정이 앱 내 이벤트, 맞춤형 제품 페이지와 제품 페이지 최적화에 용이하도록 App Store 제출 환경 출시

# Detail

- **Multiple items in one submission**
다수의 항목은 그룹화하여 단일 제출할 수 있음
- **Submit without a new app version**
새로운 앱 버전 없이도 제출을 선택할 수 있음
- **Dedicated app review submission page**
앱 전용 심사 페이지도 출시. 이 곳에서는 진행중인 제출물을 관리하고 앱 심사와 커뮤니케이션하며 최근 완료된 제출물도 볼 수 있음

## **Multiple items in one submission**

- 여러개의 맞춤형 제품 페이지나, 심사 항목을 스토어에 게시한다고 가정
제출 심사의 일부로만 항목을 제출할 수 있기 때문에 첫 단계로 할 일은 이를 제출에 추가하는 것
- 심사 제출은 앱 십사를 오가며 심사 항목을 운반하는 차량과 같음. 여러개의 항목을 그룹화하면 콘텍스트에서 모두 함께 심사받을 수 있는 장점이 있음 이를 통해서 일관성있고 효율적인 심사가 가능
- 심사가 끝나면 각각의 항목은 수락 또는 거절로 표시됨
- 항목이 하나라도 수락되지 않으면 모든 항목이 승인되지 않음

![Untitled](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/8a645c74-b5fe-4c92-9917-a6e53afcfadb)

- 제출물중 거절된 항목이 있는 경우 계속 진행하는 방법은 두가지가 있음
1. 거절된 심사 항목을 편집하여 재제출

![Untitled 1](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/08ee9840-4273-44d1-ba3b-62200927311c)

    
2. 거절된 항목을 삭제
    
    
    ![Untitled 2](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/227839e8-a279-4f4f-93c5-de989bfa433d)


### **Review submission items**

- 앱 버전
- 앱 내 이벤트
- 맞춤형 제품 페이지
- 제품 페이지 최적화 테스트

위 항목들을 심사 제출할 수 있음

## **Submit without a new app version**

- 각 제출은 연계 플랫폼 을 가지고 각각의 플랫폼은 특정 심사 항목 세트를 지원함
- 
![Untitled 3](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/1804873d-eb1b-447e-ae31-1edb83fc05b1)
![Untitled 4](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/6cce12a3-0cd1-466b-922d-f7ea96a2c379)



- 대부분의 항목은 iOS 제출의 일부로 심사되고 그룹화됨
- 플랫폼 당 진행중인 심사 제출은 하나만 가질 수 있음

![Untitled 5](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/f8d7e77e-99bb-4032-8591-e489b6a9d452)

- 이건 세개 플랫폼 버전을 모두 제출하려는 경우임

그래서 새로운 버전 없이 어떻게 제출이 가능한가?

- iOS 심사 제출을 자세히 살펴보면, 앱 심사는 제출물에 있는 모든 항목을 앱 버전과 비교 심사하여 모든 항목이 일치하는지 확인
- 제출된 앱 버전이 심사에 사용된 버전인지 확인
- 앞서 말했듯 새 버전을 추가하지 않고 제출할 수 있음. 이렇게 하려면 이전에 승인된 버전의 앱이 필요함.
![Untitled 6](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/4933d0ab-190e-4b9a-920e-1c209a657dd4)



- 앱 내 이벤트와 맞춤 페이지, 제품 페이지 최적화 테스트를 첫 iOS 버전이 승인된 후 언제나 앱 바이너리 없이 제출 할 수 있음

## **Dedicated app review submission page**

앱 스토어 커넥트의 앱 심사 제출 페이지에서 제출 개요를 볼 수 있고 상세한 내용도 확인할 수 있음

![Untitled 8](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/b6a03d8e-1b4a-4745-9d03-8a725e52e916)


- 웹 UI를 사용하는 것도 좋지만, 이동 중에도 볼 수 있으면 좋지 않을까?ㅎ 그래서 향상된 제출 환경은 iPadOS와 iOS에서도 앱 스토어 커넥트를 통해서 경험할 수 있음

![Untitled 7](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/fc545e14-1ec1-4dde-b0fb-14ba46af1a23)


- 이미 심사 준비를 끝낸 제출물은 한번의 탭으로 제출가능함
- 제출 후에는 심사 진행률도 추적이 가능함

## App Store Connect API

- 앱 스토어 커넥트 api를 통해 워크플로우를 커스텀하고 자동화할 수 있음
- 대규모 2.0 출시를 앞두고 API 리소스 수를 60% 까지 확장하고 있음
- 올 가을부터 XML feed 를 폐기하기 시작할 것. 그러므로 App Store Connect API 와 통합을 얼라인하기를 권장

기능 소개

- **In-app purchases and subscriptions**
    - 앱 내 구매 및 구독 라이프사이클 관리를 전체적으로 할 수 있음
    - subscription 을 자체 리소스로 분류하고 앱 내 구매 또는 구독을 생성, 편집, 삭제 할 수 있는 모든 권한을 부여
    - 가격을 관리하고 심사를 위해 제출하고, 특별 오퍼와 프로모션 코드를 생성할 수 있음
- **Customer reviews and developer responses**
고객의 평가와 개발자의 응답을 할 수 있는 기능 추가
- **App hang diagnostics(앱 중단 진단)**
    - 지금까지는 API 를 통해서 앱의 hang rate(중단률) 과 같은 측정 기준을 볼 수 있었지만 올 여름부터 app hang 에 대한 새로운 diagnostic type 이 추가됨
    - 기존 diagnostic signatures resource 와 함께 사용하여 앱에서 가장 많이 중단되는 위치를 찾을 수 있음
    - 뿐만 아니라 diagnostic logs relation ship 을 통해 상세한 추적도 볼 수 있음
