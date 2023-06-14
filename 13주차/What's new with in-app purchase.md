# **What's new with in-app purchase**

# **StoreKit 2**

- 2021년 공개된 StoreKit2
- 프레임 워크에 몇가지 기능이 추가됨
- 서버 사이드에서는 이런 신규 StoreKit 기능들을 완전히 새로운 App Store Server 엔드포인트들로 보완함

## 앱의 구매를 확인해주는 **App Transaction API**

- **유료 앱**에서 **앱 내 결제를 제공하는 무료 앱**으로 전환할 때 유용
- 어떤 고객이 사전 주문 pre-order 했는지
- 언제 구매했는지
<img width="901" alt="스크린샷 2023-03-05 오후 8 10 00" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/8d5c2099-9efe-4dd2-9618-a0232cdaea6f">
<img width="1398" alt="스크린샷 2023-03-05 오후 8 10 42" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/2252160c-4748-44a3-8040-9d2260462b63">


- 앱을 처음 다운 받았을 때 버전을 알려주는 `originalAppVersion`과 무료 앱 버전을 비교해서 구분
![Untitled](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/939b0f62-a1e3-483e-b980-e4f48f4fe8af)



## **New properties**

- StoreKit2에 추가된 새로운 프로퍼티들
- iOS 15+, Xcode 14+
- `price locale` : 구매 가격을 보여주는 `displayPrice` 속성이 이미 있지만, `price Locale`을 사용해서 가격의 숫자 포맷을 다양하게 정할 수 있음
    - 사용자에게 월간 구독 대신 연간 구독을 구매하면 얼마나 절약할 수 있는지 보여주고 싶은 경우
    - 매달 연간 구독료를 얼마나 내야 하는지 등
![B7F2A064-FADE-49A2-B6FB-EDB159101195](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/bf91c910-e744-42a3-a529-46e77d2b98a1)

- `environment` : 트랜잭션이 프로덕션 또는 샌드박스 환경에서 처리되었는지 여부
- `recentSubscriptionStartDate` : 최근의 연속 구독 기간을 제공 (구독 사이의 기간이 60일을 초과하지 않는 경우 연속이라고 판단)

- **오래된 OS에서 Xcode로 테스팅**할 때 위에 프로퍼티들은 `Sentinel Values`을 가짐
   
<img width="1383" alt="스크린샷 2023-03-05 오후 8 38 07" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/27cc837b-b31f-47ec-8271-c9253d38fbea">

![what](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/e10da25c-1e2f-4a1b-9d6b-3c3c2f487b32)

- 센티널 값을 통해 실제 값의 부재를 표현

# ****SwiftUI API****

- 앱 내에서
    - 프로모션 코드 사용
    - App Store 리뷰를 요청
    - 하는데 사용할 수 있는 두 가지 새로운 메소드 추가
- iOS 16+

### 제안 코드(프로모션 코드) 시트

- 코드의 만기일이나 기한 지정 가능
- 
![582BB581-0E44-4501-B84E-B728EDA0BE87](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/a134837b-5d82-490b-ae5e-21521a2c7f09)
![4001AFFC-04C4-4576-9FBB-F24133322D30](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/0e587a27-7ee3-4576-a25a-3ebe4f0cf8d3)



### 이제 SwiftUI 에서도 앱스토어 리뷰 요청을 할 수 있음
![image](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/f16be8ad-53ec-4674-a996-29f33e969f17)


- 365일 기간 내 최대 3회만 권장
- 자주 묻지 말고 **같은 버전의 앱**을 여러 번 검토하게 하지않도록
- 고객 상호 작용을 방해하지 않도록

# StoreKit Message

- Apple은 이메일을 통해서만 가격 인상 계약을 알릴 수 있었음
- 이 새로운 StoreKit 메시지 API를 통해 앱에서 바로 앱 구독자에게 메시지를 직접 보낼 수 있음
- 기본적으로 StoreKit 메시지는 사용자가 앱을 포그라운드로 가져올 때 앱 위에 표시
- 그러나 이러한 메시지가 앱의 중요한 흐름을 방해하게 하고싶지 않다면 리스너를 구현하면 됨

<img width="948" alt="스크린샷 2023-03-05 오후 8 54 29" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/21360582-3072-4e96-93a9-861d4646fe5c">
<img width="1122" alt="스크린샷 2023-03-05 오후 8 55 25" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/2fdd41d1-2add-4843-be96-8fc129d9e632">


# ApplicationUsername

- `applicationUsername`은 트랜잭션을 서비스의 사용자 계정과 연결하기 위해 생성하는 문자열
- StoreKit이 모든 StoreKit 트랜잭션에서 이 정보를 보존할 수 있도록 사용자의 UUID를 전달해야 하는 것을 권장
- 또한 App Store Server Notifications v2에서 appAccountToken으로 이 정보를 받을 수 있음
- [https://example.com/v2](https://example.com/v2)
![image1](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/d96c7d60-511b-4d67-8786-31e77553c501)

