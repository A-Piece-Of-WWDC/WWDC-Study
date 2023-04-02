# ****Triage test failures with XCTIssue****

- Xcode 12에서 등장하게된 새로운 Test용 API 소개 세션
- 실패하는 테스트에 대한 디버깅 작업은 우리의 프로젝트 테스트 세트를 유지 보수하는 과정에서 가장 중요한 작업 중의 하나이다
- 사실 우리의 테스트가 왜 실패하는지에 대한 분석과 진단 과정은 꽤나 시간이 소요되는 작업이다
- 실패의 원인은 다양할 수 있는데, 그 중 하나로 계속해서 성장하고 있는 프로젝트일 경우, 코드 변경으로 인해 테스트가 실패할 수 있음
- 이렇게 테스트가 실패할 경우, 우리는 다음과 같은 의문이 생기게 된다
    - What failed?
    - How did it fail?
    - Why did it fail?
    - Where did it fail?

- 새로운 테스트 API는 위 의문들을 좀 더 효율적으로 해결해줌. 이 세션에서는 다음 4가지를 통해 위 의문들을 해결해나가볼 예정
    1. Swift errors in tests
    2. Rich failure objects
    3. Failure call stacks
    4. Advanced workflows

## **Presenting Test Failures in Xcode 12**

- Xcode 12에서는 다음과 같이 테스트 실패에 대해 표현된다

![스크린샷 2023-03-05 오후 7 36 31](https://user-images.githubusercontent.com/87598209/229332150-dbcf7dc5-d256-446d-b49e-cb0e3742440f.png)

- 위 예시 코드를 보면, 25번 라인에 회색으로 표시되어 있는 것을 확인할 수 있음. 이건 이 라인을 콜하는 과정에서 에러가 발생했다는 의미이다

![스크린샷 2023-03-05 오후 7 41 15](https://user-images.githubusercontent.com/87598209/229332154-e2d932fa-8f82-4491-bdc3-902f0a54d07a.png)

- 테스트 실패에 대한 원인을 파악하기 위해 Issue navigator의 call stack을 확인하자
    
    ![스크린샷 2023-03-05 오후 7 42 09](https://user-images.githubusercontent.com/87598209/229332156-be46e741-747b-4919-9efc-875228e706b7.png)

- 가장 마지막 콜을 확인해봤을 때, 19번 라인이 빨간색으로 표시된 것을 확인할 수 있음. 이는 실질적인 테스트 실패에 대한 원인이된 코드라는 뜻이다
- 이를 통해, 우리는 에러가 어디서부터 어떤 원인으로 시작됐는지를 파악할 수 있음

![스크린샷 2023-03-05 오후 7 44 59](https://user-images.githubusercontent.com/87598209/229332159-950b15e7-bbdb-4952-916d-f2e05c5f34bd.png)

- Report Navigator에서 똑같은 내용을 확인할 수 있다

# **Swift errors in tests**

- Swift로 작성된 error를 우리들의 테스트에 활용해보자
- 우리가 작성한 Swift 에러를 통해 우리는 테스트 실패에 대해 좀 더 분명하고 명확하게 이해할 수 있다.
- 또한, Swift 에러를 사용하면 보일러 플레이트도 줄일 수 있다.
    
    ![스크린샷 2023-03-05 오후 8 11 01](https://user-images.githubusercontent.com/87598209/229332163-4e73c9ec-35b2-4dd9-ab1b-116a38261505.png)

- 이런 코드를 아래 처럼 줄여줄 수 있다
    
    ![스크린샷 2023-03-05 오후 8 11 17](https://user-images.githubusercontent.com/87598209/229332168-e9a7e827-0ff2-4fce-8d8f-6d809b987cd7.png)


## Throwing from setUp and tearDown

- test function에 throw가 생긴 위의 변화처럼 setUp과 tearDown 메서드에서도 변화가 생겼다
- 새롭게 setUpWithError 메서드와 tearDownWithError메서드가 등장하였음
    
    ![스크린샷 2023-03-05 오후 8 17 23](https://user-images.githubusercontent.com/87598209/229332173-b9c6813a-72f5-4777-b0d2-45cc8c4e971e.png)

- 새롭게 추가된 두 메서드는 기존 메서드와 약간의 차이만 있음
- 이제 setUp과 tearDown과정에서 에러를 throw 할 수 있음
- setpUp과 setUpWithError메서드 둘다 사용하는 것도 가능하나 가급적이면 setUpWithError만 사용하는 것을 권장(tearDownWithError도 동일)

# Rich failure objects

- XCTest는 테스트 실패에 대해 4개의 별개의 값들로 기록해둔다
    - Failure message
    - File path
    - Line number where the failure was recorded
    - "Expected" flag to indicate if the failure was expected.
        - Expected 값은 XCTAssert를 통해 결정되어짐
- 이 값들은 recordFailure 메서드를 통해 전달되어진다.
    
    ![스크린샷 2023-03-05 오후 8 25 04](https://user-images.githubusercontent.com/87598209/229332176-14aa5775-c9f6-4094-a5d9-9e421785bc6b.png)


- Xcode 12에서는 이 4가지 별개의 값들을 캡슐화한 새로운 객체 XCTIssue가 등장하게됨

![스크린샷 2023-03-05 오후 8 25 44](https://user-images.githubusercontent.com/87598209/229332181-a50425e1-a2dc-43b9-a917-d9af34bd0c5c.png)

- 또, 거기에 더해서 3가지 새로운 종류의 실패 데이터들이 추가됨
    - Distinct types
    - Detailed description
    - Associated error
    - Attachments
- XCTAttachment는 임의의 데이터들을 저장해둘 수 있음
    
    ![스크린샷 2023-03-05 오후 8 32 46](https://user-images.githubusercontent.com/87598209/229332185-e2662221-ff87-4ef9-9e43-92a4829e65a8.png)

    ![스크린샷 2023-03-05 오후 8 32 58](https://user-images.githubusercontent.com/87598209/229332187-c1827d75-8759-4fc5-93c8-365e80353663.png)

    - 이런 느낌
- XCTIssue는 attachment를 지원하고 있음, 따라서 attachment를 통해 우리는 테스트 실패에 대해 커스터마이즈된 진단이 가능하게됨
- 또한 테스트 실패에 대한 기록을 위해, 앞서 설명했던 recordFailure이 발전된 형태인 record 메서드가 등장하게 되었다(recordFailure 메서드는 deprecated 됨)
    
    ![스크린샷 2023-03-05 오후 8 36 57](https://user-images.githubusercontent.com/87598209/229332190-8a39f3ac-b561-4919-8107-6da9390ac648.png)


# **Failures and Source Code**

- 테스트 실패에 대해 가장 중요한 질문 중의 하나가 ‘Where did it happen?’ 이었음
- 기존에는 에러가 어느 라인에서 시작된 건지 확인하기 힘들었음
    
    ![스크린샷 2023-03-05 오후 8 44 58](https://user-images.githubusercontent.com/87598209/229332193-ebbb6a4f-3fa4-4b4a-9ded-a0f2f61256f4.png)

- 이런 식으로 에러의 원인이 되는 라인에만 에러가 표현되었음
- 따라서 에러가 어디서부터 시작됐는지 파악하기 위해서는 다음과 같이 메서드를 만들어서 사용해줘야 했음
    
    <img width="1498" alt="스크린샷 2023-03-09 오후 6 31 00" src="https://user-images.githubusercontent.com/87598209/229332196-be66c41c-bf59-4611-a286-d74dd54b14d4.png">


- 그러나 이제 Xcode 12에서는 이러한 과정이 불필요해졌음
    
    <img width="1413" alt="스크린샷 2023-03-09 오후 6 32 18" src="https://user-images.githubusercontent.com/87598209/229332200-6de47e14-f672-4efc-8af4-e28acd57b6ce.png">

- 이제 우리는 콜스택을 확인함으로써, 에러가 어디서부터 시작됐는지를 확인할 수 있다 !
- 제일 좋은건, 이러한 콜스택이나 결과를 얻기 위해 우리는 어떤 추가적인 노력이 필요없다는 것

# **Advanced Workflows**

- 테스트 실패에 대한 좀 더 명확한 진단 방법 중의 하나는 기존의 워크플로우를 커스터마이즈 하는 것

![스크린샷 2023-03-05 오후 8 40 55](https://user-images.githubusercontent.com/87598209/229332205-22247bce-1ad7-4d8f-b54a-a68e3cecf530.png)

- 위 예시는 CustomAseertion메서드를 만든건데, 만약 data가 유효하지 않을 경우 XCTIssue를 만들어서 테스트 실패에 대한 정보들을 좀 더 상세하게 기록하고 있는 것을 확인할 수 있다.
- XCTAttachment도 활용하고 있는걸 볼 수 있다
- 또 다른 방법으로는 record를 오버라이딩 하는 방법도 있음 !
![스크린샷 2023-03-05 오후 8 54 32](https://user-images.githubusercontent.com/87598209/229332209-9502aa32-e77d-428a-80ed-efbe574d9c70.png)

