# ****Meet AsyncSequence****

## What is AsyncSequence?

![스크린샷 2023-04-02 오후 3 58 10](https://user-images.githubusercontent.com/87598209/231491112-118b17be-8ff8-4dfb-a290-df61a7ebdc0b.png)

- 각각의 비동기 작업이 처리될 때마다 아래의 for 문을 iterating하게 됨(기존의 for-in loop와 똑같은 방식, 비동기 작업을 처리하냐의 차이)
- 가령, 무언가 용량이 많은 데이터들을 다운로드 받아서 처리해야 한다면, 각각의 데이터가 다운로드될 때마다 처리해주는 방식으로 사용할 수 있음
- 가장 매력적인 점은, 우리가 익숙하게 사용하고 있는 for문의 형태를 비동기 작업에서도 사용할 수 있다는 점임
    - for-await-in문을 통해서 사용 가능. 만약 throwable한 비동기 작업의 경우, for-try-await-in 문을 통해서 사용 가능
- dropFirst메서드뿐만 아니라 map, filter, reduce 등의 메서드와 함께도 사용 가능

### async/await Key Point

AsyncSequence를 본격적으로 들어가기 전, 몇 가지 async/await 키 포인트 복습

- async 메서드는 await 키워드를 통해서 ****비동기 작업을 마치 동기 코드처럼 작성할 수 있게 만들어줌.
    - 일반적인 비동기 작업 처리 메서드처럼, 콜백이 전혀 필요없음
- async메서드는 await키워드를 만나면 언제든지 잠시 중단되었다가 비동기 작업이 끝나거나 에러를 얻게 되면 다시 resume됨
    - AsyncSequence의 경우, 각각의 element를 iterate할 때, 잠깐 중단되었다가 재개되는 형식

![스크린샷 2023-04-02 오후 4 39 49](https://user-images.githubusercontent.com/87598209/231491177-46bb913f-661a-4f87-bb55-86cabc5f49ae.png)

### AsyncSequence Key Point

- async 키워드만 제외하고 일반적인 Sequence와 거의 동일. 각각의 element가 비동기 형태로 전달되는 방식
- 일부 AsyncSequence는 error를 throw할 수 있음
- AsyncSequence는 iterating이 모두 끝나거나 error가 throw되면 종료됨

### 일반적인 Sequence

![스크린샷 2023-04-02 오후 4 41 24](https://user-images.githubusercontent.com/87598209/231491228-4e120600-f4b6-4615-8338-36703e4aa3c9.png)

- 우리가 익숙한 for-in loop문 형태
- 우리가 사용할 때는 위와 같이 사용하나, 컴파일러 입장에서는 아래와 같이 iteration를 다루게 됨
    
    ![스크린샷 2023-04-02 오후 4 42 20](https://user-images.githubusercontent.com/87598209/231491255-7c0b15e1-9cde-4ad5-b0e5-d85b61e4c262.png)

- iterator를 만든 뒤 next메서드를 이용하여 while문을 돌리는 형태

### AsyncSequence

![스크린샷 2023-04-02 오후 4 48 11](https://user-images.githubusercontent.com/87598209/231491271-b7e5eea4-dcb7-4b1f-be49-fb0d98ff5e6d.png)

![스크린샷 2023-04-02 오후 4 46 29](https://user-images.githubusercontent.com/87598209/231491288-356578a1-21fa-470a-a876-a79c7a6a7860.png)

- 일반적인 Sequence와 약간의 차이가 있음. AsyncIterator를 만들고 똑같이 next메서드를 사용하는 방식

## Usage and APIs

![스크린샷 2023-04-02 오후 4 52 29](https://user-images.githubusercontent.com/87598209/231491304-d0b67ddd-ec10-4194-83cc-f8f822b939a2.png)

- 각 item의 특정 property에서 nil이 발견되면 iterating을 중단시키는 방식으로 활용 가능

![스크린샷 2023-04-02 오후 5 01 24](https://user-images.githubusercontent.com/87598209/231491323-1f3a5fc6-0141-467f-9a7b-33094e98aefb.png)

- 또는 property의 값에 따라서 특정 item을 skip하고 싶다면, 위와 같이 continue 활용도 가능

![스크린샷 2023-04-02 오후 5 03 14](https://user-images.githubusercontent.com/87598209/231491350-e5e96048-15a6-4598-bb4f-abd7cd659054.png)

- 만약 error를 throw하지 않는 AsyncSequence라면 for-await-in 문을 쓰면 되지만, error를 throw한다면 do-try-catch문을 함께 활용해서 for-try-await-in 문을 써야함
- 만약 try를 안쓴다면 컴파일러가 알아서 지적해줄거임. 이 말은 코드를 작성하는 우리의 입장에서 언제나 안전하다는 뜻. 즉 실수로라도 try를 안 빼먹게 해주니까?

![스크린샷 2023-04-02 오후 5 07 11](https://user-images.githubusercontent.com/87598209/231491371-29b1a527-9e18-4c5a-8cfe-5ce410cfa5d9.png)

- Task를 활용하여 우리의 iteration 작업을 캡슐화해줄 수도 있음
- 그러나 이렇게 사용할 경우, 간혹 비동기 작업이 지연되어서 error도 throw하지 않고 중단되지 않는 경우가 생길 수도 있음

![스크린샷 2023-04-02 오후 5 11 57](https://user-images.githubusercontent.com/87598209/231491390-837bfc4b-b9fc-4156-8492-904d3db1bddd.png)

- 앞서 말한 중단되지 않는 경우를 대비해서, 우리는 Task클로저를 프로퍼티에 담아서 cancel메서드를 활용해줄 수 있음
- 즉, 외부에서 비동기 작업을 통제해주는 형태
