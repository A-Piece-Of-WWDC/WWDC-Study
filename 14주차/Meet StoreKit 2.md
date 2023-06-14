# Meet StoreKit 2

- Meet StoreKit2 세션은 클라이언트 측 기능 및 구현에 중점
- StoreKit2는 iOS, macOS, tvOS 및 watchOS에서 인앱 구매를 위한 최신 Swift API
- async/await 패턴을 사용하는 Swift concurreny 지원
- 인앱 구매 거래에 대대적인 업데이트 → 더 많은 정보와 높은 보안을 제공하는 동시에 더 쉽게 작업할 수 있도록
- 구독을 위한 API 추가
- StoreKit2 API는 기존 StoreKit 프레임 워크 내에 있음

![Untitled](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/9378e6fa-2029-47b9-b6a9-8948807a6aad)


- 새로운 StoreKit2는 `제품`, `구매`, `거래 정보`, `거래 내역` 및 `구독 상태`의 5가지 주요 영역으로 구성

# 💰 Product and Purchase

![image](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/f453beb9-7478-48db-8db9-aae87783fe35)
<img width="595" alt="스크린샷 2023-03-15 오후 6 18 36" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/f2b44c2f-0d30-429c-b132-a5e2fb890f60">



- StoreKit 2 제품 구조는 기존 StoreKit 제품 객체가 강화된 버전
- 제품 유형 및 구독 정보와 데이터가 추가됨
- StoreKit 2에서는 구독 정보를 이용해 고객이 구독 제안을 받을 수 있는지를 쉽게 확인할 수 있음
- `BackingValue`라는 래핑 유형을 추가하여 최신 버전의 StoreKit에서도 이전 버전의 데이터에 접근할 수 있도록 함

![123414](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/2d53f3e4-0d44-45fc-a118-4d30e4cfd0a9)


- StoreKit 2에서는 정적 함수를 호출하여 제품을 요청하면, 기존 SKProductsRequest와 동일하게 App Store에서 제품 메타 데이터를 요청
- async/await 패턴으로 **제품 요청과 구매**가 한 줄의 코드로 작성될 수 있음
- 구매는 이제 제품 객체의 메서드이기 때문에, 방금 검색한 제품을 가져와서 직접 구매를 호출할 수 있음
- 디폴트 세팅 이상으로 구매 행동을 수정하려는 경우 `구매 옵션` 설정을 통해 가능함
   
   ![315135](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/ca436dde-23c1-4fbe-a6b4-92cd46ed9447)

   
    - 구매 옵션은 구매의 단일 속성을 설명하는 항목
    - 수량, 할인(프로모션 제안) 등의 속성을 결정할 수 있음
    - 어떤 사용자 계정이 거래를 시작하고 완료했는지 추적할 수 있는 **앱 계정 토큰**이 추가됨

![1324135](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/5df927e3-d65a-4273-a338-c9bf07c44352)


- **앱 계정 토큰**을 통해 트랜잭션을 시작하고 완료한 앱의 사용자 계정을 추적할 수 있음
- UUID 형식을 준수하기만 하면 돼서 쉽게 생성할 수 있음
- 구매 옵션으로 구매 시 **앱 계정 토큰**을 보내면 이 토큰이 해당 구매에 대한 거래 정보에 포함됨
- **앱 계정 토큰**은 모든 기기의 거래 정보에 영원히 유지됨

- 구매가 완료되면 StoreKit은 암호화 서명된 정보와 함께 성공적인 트랜잭션을 반환함

# 💰 Transaction Info
![asdf](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/35c5b779-a326-409c-bb6d-d8bd4c5f4391)



- 구매가 처리되면 StoreKit 2는 **모든 트랜잭션에 대해 개별적으로 서명된 객체**를 제공
- 웹에서 공통 표준으로 사용되는 JSON 웹 서명을 사용한 JSON형식으로 제공
- 서명된 객체에 포함된 모든 정보는 기본 StoreKit API를 통해 사용할 수 있기 때문에 쉽게 데이터를 이용해 작업할 수 있음

### 예시 앱 Pocket Cars

![wrgwa](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/434c2dac-2274-4767-829a-eca0202a4d08)

- Xcode 프로젝트에서 판매하려는 제품을 정의하는 StoreKit 구성 파일을 만들 수 있음

![e](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/f5ab4d9d-8c28-4b57-b5e0-2a868d804f98)


- 각각의 제품을 구분하기 위해서 제품 식별자가 포함된 plist

![sdv](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/6762c0a7-3d52-4bf7-b9a0-d10b38ffbe87)


- StoreKit 2를 사용하면 Product 구조체의 request 정적 메서드를 통해 쉽게 상품 정보들을 가져올 수 있음
- 제품 정보에 포함된 type이 App Store 서버에 정의된 type에 대한 속성을 제공하기 때문에 쉽게 구매 유형을 분류할 수 있음
- consumable은 한번 사용하면 없어지는 형태의 제품 ex. 연료
- nonConsumable은 한번 구입하면 영원히 소유하는 속성의 제품 ex.자동차
- autoRenewable은 가입 후 정기적으로 요금이 청구되며, 원할 때 업그레이드나 다운그레이드 할 수 있는 제품 ex.내비게이션 패키지?
- sortByPrice(_: [Product]) 메소드를 통해 제품을 가격순으로 정렬할 수 있음
![dvsa](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/78981421-84bb-4b0e-8ba3-77788d90b5e3)



→ 한줄의 코드로 앱 제품을 요청할 수 있고, 받은 메타 데이터를 기반으로 제품을 그룹화, 정렬할 수 있다.

- StoreKit2는 처음부터 Swift의 새로운 동시성 기능을 사용하기 위해 구축됨!
- 구매를 할 때에는 해당 제품 객체에서 purchase를 호출하면 새 동시성 기능을 통해 `PurchaseResult`가 반환됨
- `PurchaseResult`를 통해 구매가 성공/취소/구매를 위해 은행의 추가 확인 등의 별도 상태로 전환되었는지 여부를 알려줌

- PurchaseResult가 성공 상태인 경우에 확인 상태 verification를 받음
- 이 예제에서는 checkVerified함수를 통해, 결과가 확인되지 않으면 자체 failedVerification 오류가 리턴
- 사용자가 콘텐츠를 얻은 후에는 StoreKit의 트랜잭션을 완료
- purchase함수를 호출할 때, appAccountToken을 옵션에 전달하여 현재 로그인한 사용자에 대한 토큰을 전달할 수 있음

![wegwrg](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/b1c605bb-2fb6-49c3-9d1d-cb64effd77b7)


- 때때로 고객은 구매가 일어날 때 부모의 승인과 같은 추가적인 처리가 필요
- 이러한 경우 product.purchase()의 결과는 보류 상태가 됨
- 이러한 트랜젝션을 처리하기 위해서는 이후에 StoreKit에서 발생하는 이벤트를 무한 비동기 시퀀스를 통해서 지속적으로 확인해야 함
- 여기서는 거래의 결과가 오면 이전의 처리와 마찬가지로 기존에 구현한 checkVerified를 통해 결과가 확인되었는지를 검증하고 updatePurchasedIdentifiers를 통해 UI를 업데이트하고 트랜젝션을 마침

- 방금 구현한 업데이트 리스너 테스트를 위해, Product 파일에서 Ask To Buy를 활성화하면 대기중인 구매 응답을 시뮬레이션할 수 있음

![eath](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/b1b6fe4c-a15e-49a4-bb99-ea56d0512762)



- 이제 결제 사이트에서 구매를 하면 권한을 요청하는 팝업이 표시됩니다.

![asd](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/de8b17cf-3779-4850-a81d-662086a4ce66)

- 테스트를 진행하기 위해서는 하단의 Xcode의 버튼을 눌러 StoreKit 테스트의 트랜잭션 관리자를 열 수 있습니다.

![erg](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/628223d0-fb2c-40db-b9ab-9e842a560540)


- 해당 거래를 선택하고 우측 상단의 승인 버튼을 클릭하면 이전에 만든 트랜젝션을 처리하는 함수가 수행되고 UI가 업데이트됩니다.

# 💰 Transaction history

<img width="724" alt="wergeg" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/5233cd2a-f0b4-41c8-a313-ff2ac3b68d03">

![asdfasdf](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/54a59567-3ef4-41b9-a596-a21b9bda1fac)


- 사용자의 거래 내역에서 완료된 거래를 얻기 위한 새로운 API가 추가
단일 호출로 사용자의 모든 과거 트랜잭션에 액세스할 수 있음
- 또한 가장 최근 트랜젝션에 액세스할 수도 있음
- 현재 제품 정보에 대한 거래 정보 또한 얻어올 수 있음

![qweg](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/72b9b0a5-9d12-4cd4-9d7b-e6e1475301b4)


- CurrentEntitlements 함수를 통해서 사용자의 거래 내역의 모든 **비소모품과 현재 활성화된 모든 구독 거래**를 얻어올 수 있음
- 취소된 항목은 포함되지 않으므로 사용자가 현재 구매하여 활성화된 모든 정보를 얻어올 수 있게 됨
- 소모품은 거래 내역의 영구적인 요소가 아니기 때문에 때문에 포함되지 않음

![zz](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/d9b99e6f-c8f5-42ac-8a5a-239a402989c0)


- StoreKit 2를 사용하면 사용자가 새 기기에 앱을 설치할 때 바로 내역을 얻어올 수 있게 됨
- 고객이 한 기기에서 구매하면 앱이 설치된 다른 모든 기기에서 구매내역을 확인할 수 있음
- 실제로 다른 기기에서 구매할 때 앱이 실행 중인 경우 실시간으로 새 거래에 대한 알림을 받게 됨.. 신기하다
![qwef](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/55c74f95-fee3-47f8-9a0a-173a1b98a42a)



- 드물게 사용자가 구매를 했다고 생각하지만 표시되지 않는 경우 App Store 동기화 API를 사용하여 모든 StoreKit 2 트랜잭션을 **즉시 재동기화**할 수 있음
- 이는 `restoreCompletedTransactions API`를 대체하며, 사용자가 동기화를 시작할 수 있도록 하는 UI를 앱에서 제공해야 함
- 사용자가 수동 동기화를 시작해야 하는 경우 계정을 인증해야 하기 때문에, 사용자 입력에 대한 응답으로만 이 API를 사용해야 함

![refqwf](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/4acc9cd7-3096-4c9a-8492-0511754923e7)


- StoreKit 2 API를 사용하여 이루어진 트랜잭션은 원래 StoreKit API에서 사용할 수 있으며 그 반대의 경우도 마찬가지
- 앱에 기존 트랜잭션이 있는 경우 즉시 StoreKit 2 API에서 볼 수 있음
- 통합 영수증 내에서도 StoreKit 2로 구매한 것을 사용할 수 있음

# 💰 Subscription status

![wqef](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/a2df19cc-5d3f-4178-8bfe-0607738f5a69)
<img width="745" alt="caca" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/d0bb4fb9-29e5-49a9-9f9a-dd3a731d16dd">


- `**구독 상태**`는 세 부분으로 구성
- 첫 번째는 `최신 거래`이기 때문에 구독에 대해 발생한 마지막 트랜잭션에 쉽게 접근할 수 있음(앞에서 얘기한 최신 트랜잭션 호출하는 것과 같음)
- 두 번째는 `갱신 상태`이며, 이것은 구독의 현재 상태가 현재 구독 여부, 만료 여부, 유예 기간 등을 알려주는 열거형
- 마지막 부분은 `갱신 정보`이며, 사용자의 구독에 대한 모든 세부 정보를 볼 수 있음
    - 이 데이터는 트랜잭션 없이 변경될 수 있기 때문에 트랜잭션 정보에 없는 모든 종류의 정보를 포함
        
        ![qwefq](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/5ae8afb7-4c81-4ffd-a884-a73b5c7cd6bb)

        
        - 사용자가 자동 갱신을 켰는지 여부를 가지고 있음
        - 또한 구독에 대해 자동 갱신이 설정된 제품 ID도 확인할 수 있기 때문에 다운그레이드 한 경우 이 값을 통해 확인할 수 있음
        - 구독이 이미 만료된 경우 갱신 정보를 사용하여 만료 이유를 확인할 수 있음
    - 전체 갱신 정보에는 중요한 데이터가 포함되어 있기 때문에 JWS를 사용하여 서명됨
    - 구독 API 호출 시에는 사용자가 동일한 제품에 대해 여러 구독을 가질 수 있기 때문에 상태 배열을 반환함.

![sdaklnf](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/9ddf10d7-4c02-4fb7-b7f3-ceaca70def92)

- isPurchased 함수에 Transaction.latest(for:제품식별자)를 호출하도록 하여 가장 최근 거래정보를 받아올 수 있음
    - 이 StoreKit 메서드는 트랜잭션이 StoreKit 2의 확인 검사를 통과했음을 얻을 수 있는 결과를 반환하며, 이는 이전의 checkVerified 함수를 통해서 결과를 얻어올 수 있도록 합니다.
- revocationDate의 nil 여부를 확인하여 환불된 거래인지를 알 수 있으며, 기간 중간에 고객이 더 높은 수준으로 업그레이드한 구독은 isUpgraded 플래그가 true로 설정됨
![354rtvsze](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/767ac690-c2cd-44d0-8688-0abf4159bba4)

![7648n](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/1e296fea-32c9-48f4-b190-c01de3d22d3b)


- 위의 예에서는 updateSubscriptionStatus함수를 통해서 StoreKit에서 구독 상태를 가져와 사용자에게 표시할 수 있음
- status 속성은 배열을 반환 → 여러 구독 정보를 가지고 있을 수 있기 때문에, 가진 구독 정보를 순회하면서 제공할 수 있는 가장 높은 등급의 서비스를 얻어옴
- 구독 상태가 만료되거나 취소되었는지를 확인해야 함
![3cc3](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/70168249/0485dbc1-e931-41b3-a347-ec1b1d5f58ed)


- currentEntitlements를 호출하면 사용자의 유효한 거래를 들을 가져올 수 있음
- 결과 목록에서 확인된 목록만을 필터링할 수 있으며, 이후 갱신 가능하거나 비소모성 제품에 대해서만 필터링해서 표시할 수 있음.
