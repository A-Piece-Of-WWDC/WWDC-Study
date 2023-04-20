# ****Use async/await with URLSession****

- URLSession에 Swift의 동시성 프로그래밍을 어떻게 적용시킬지에 대한 세션

## 기존 방식 URLSession dataTask

![스크린샷 2023-04-19 오후 7 52 12](https://user-images.githubusercontent.com/87598209/233224271-3dbffbfb-d1b2-4bf4-b8ce-cbfb24a25164.png)

- 우리가 기존에 사용하는 위와 같은 방식의 코드는 여러 가지의 문제점을 발생시킬 수 있음

![스크린샷 2023-04-19 오후 7 54 22](https://user-images.githubusercontent.com/87598209/233224277-a72ff6da-1362-4279-a732-d70ed3ac2760.png)

- 위의 예시를 통해 일반적인 flow를 봤을 때, 우리는 task를 생성하고 해당 task를 실행함(resume)
- 이후 task가 완료되면 dataTask의 CompletionHandler가 실행됨
- 그 다음, 받아온 data를 통해 UIImage를 만들어주고 fetchPhoto메서드의 completionHandler가 실행되게끔 만들어주고 있음

### 문제점

1. 위와 같은 flow로 흘러갈 때, thread전환이 빈번하게 일어나게됨.
    1. dataTask메서드의 completionHandler는 백그라운드 스레드에서 동작
    2. 이미지를 생성하고 난 뒤에는 메인스레드에서 completionHandler를 넘겨주고 있음
    3. 이러한 상황에서 스레딩 이슈가 발생할 수도 있음
2. 만약 error가 발견된다면 completionHandler를 2번 실행시킬 수 있는 구조임.
3. completionHandler를 일관되게 보내고 있지 않음.
    1. 어떤 건 handler는 메인스레드, 어떤 handler는 백그라운드스레드

![스크린샷 2023-04-19 오후 8 04 55](https://user-images.githubusercontent.com/87598209/233224286-ce9b8d89-6d4f-415b-a27d-78c1b167fb68.png)

1. UIImage를 data를 통해 이니셜라이즈하는 것은 실패할 수도 있음.
    1. 그렇게되면 nil과 nil을 넘겨주게 됨. 이는 개발자가 예상하지 못한 상황일 수 있음.

![스크린샷 2023-04-19 오후 8 07 56](https://user-images.githubusercontent.com/87598209/233224293-db6e81e9-4f48-4e28-8743-26adb6d0961c.png)

## Use Async/Await

![스크린샷 2023-04-19 오후 8 09 17](https://user-images.githubusercontent.com/87598209/233224296-9a534e40-3d7d-4f29-acdb-bb565d3be7fe.png)

- async/await을 사용함으로 인해 우리는 보다 직관적인 코드를 작성 가능
- 또한 메인스레드와 백그라운드스레드를 메서드 내부에서 전환시켜줄 필요가 없음
- 스레딩 이슈로부터 자유로워짐

![스크린샷 2023-04-19 오후 8 18 30](https://user-images.githubusercontent.com/87598209/233224310-3de05c68-b21b-406d-a78b-1f5c60cb6200.png)

- 또한, 에러 핸들링도 completionHandler를 써서 넘겨줄 필요 없이 throw를 활용해주면 됨
- 앞서 언급한 문제점 중의 하나는 data를 이미지로 올바르게 변환 못시켰을 때, nil이 발견된다는 문제였는데 해당 문제또한 guard let 바인딩을 해주고, 만약 이미지에 nil이 발견된다면 에러를 throw 해주면 됨

## URLSession.data

![스크린샷 2023-04-19 오후 8 21 52](https://user-images.githubusercontent.com/87598209/233224320-f964fdff-4252-4db9-9155-6a4d4bfd73d6.png)

- 위 메서드 2개는 이번에 URLSession에 새롭게 추가된 data 관련 async 메서드
- URL과 URLRequest를 처리해줄 수 있음
- 기존 dataTask기능과 똑같은 기능 제공

## URLSession.upload

![스크린샷 2023-04-19 오후 8 32 56](https://user-images.githubusercontent.com/87598209/233224338-a7961022-cbc8-4d00-abbe-bd4daa0a6222.png)

- upload관련 async 메서드도 추가됨
- 기존 uploadTask와 똑같은 기능 제공
- 주의할 점으로 httpMethod를 꼭 체크해줘야함. upload메서드는 default httpMethod인 GET을 지원하지 않음

## URLSession.Download

![스크린샷 2023-04-19 오후 8.35.23.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e286fc73-b6c8-4d87-84cc-f042350b4baf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-04-19_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.35.23.png)
![스크린샷 2023-04-19 오후 8 35 23](https://user-images.githubusercontent.com/87598209/233224346-a303c306-1227-4001-b656-da8df07653db.png)

- download관련 async 메서드도 추가됨
- 기존의 downloadTask와 차이점은 async 메서드의 경우, 불필요 해졌을 때, 파일을 자동으로 삭제하지 않음. 따라서, 사용자가 직접 삭제해줘야한다는 것을 잊지 말아야함

## Cancellation

![스크린샷 2023-04-19 오후 8 41 44](https://user-images.githubusercontent.com/87598209/233224362-042152ed-914a-4862-aeae-b4a28610ee61.png)

- Task 블럭과도 함께 활용해줄 수 있음. 우리는 Task블럭에 async메서드를 처리하고 작업이 완료되거나 작업을 중단해야할 경우, Task의 cancel 메서드를 사용해주면 됨.
- 주의해야할 점은 Swift Concurrency의 **Task**와 **URLSessionTask**는 이름은 비슷하지만 서로 전혀 관련이 없음
