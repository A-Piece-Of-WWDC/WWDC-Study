## LLDB(Low-Level Debugger)란

- LLDB를 이해하기 위해서는 LLVM에 대한 이해가 필요
    - LLVM(Low-Level Virtual Machine)
        - Apple 에서 진행한 Compiler에 필요한 Toolchain 개발 프로젝트
            - 여기서 툴체인 이란?
                - 프로그램 개발을 위한 개발 도구들(ex. 컴파일, 빌드, 디버깅 등등)
        - 컴파일을 함에 있어서 필수적인 라이브러리이다
        - 고수준의 Swift, 옵씨 등의 언어를 기계어로 바꿔준다
        - LLVM의 서브 프로젝트로 LLVM의 Debugger Component를 개발하기 위해 수행된게 바로 **LLDB**이다.
- 정리하면, LLDB는 LLVM을 위해서 개발된 디버깅 툴.
- Xcode는 default 디버깅 툴로 LLDB를 사용하고 있다
- 런타임시 발생하는 오류들을 잡는 데에 용이하다
    
    ![스크린샷 2023-01-05 오후 12 39 27](https://user-images.githubusercontent.com/87598209/211025247-d7d7cc7f-216c-493b-b4d5-f17c4bcdcbfa.png)


## LLDB를 잘 사용했을 때의 이점

- 현업에서 프로젝트 빌드 시간은 상당히 오래걸림(회사 프로젝트 기준 클린 후 빌드 - 약 1분~1분 30초 되게 빠른편,,)
- 한 번의 빌드로 최대한 많은 정보를 파악할 수 있다
- 즉, 효율성이 증대되고 불필요한 빌드를 추가로 할 필요가 없으므로 시간 낭비를 줄일 수 있다

## “po”

- LLDB는 디버깅을 위한 다양한 종류의 커맨드를 제공하고 있다
- 변수를 콘솔에 출력하기 위한 대표적인 커맨드 중 하나인 ‘po’가 있음
    
    ![스크린샷 2023-01-05 오후 4 16 57](https://user-images.githubusercontent.com/87598209/211025263-6cabf6e4-db0d-4231-a8a9-c1180f42d074.png)

- po를 사용하면 해당 객체의 description을 받아서 보여줄 수 있음
- description을 우리가 원하는 내용으로 customize도 해줄 수 있음
- 단 커스텀할 경우, 해당 타입(Trip)이 CustomDebugStringConvertible 프로토콜을 준수해줘야 함

![스크린샷 2023-01-05 오후 4 20 41](https://user-images.githubusercontent.com/87598209/211025277-3ff10f25-8631-4189-947e-14198116aa82.png)

- description으로 내가 지정해둔 String값을 가져온 것을 확인할 수 있음
- 이렇게 커스텀한 description은 top-level description에만 적용이 됨
- 만약 서브구조의 description을 커스텀해주고 싶다면 CustomReflectable 프로토콜을 준수해줘야 함

![스크린샷 2023-01-05 오후 4 23 38](https://user-images.githubusercontent.com/87598209/211025287-a0955ee4-5c55-4b03-8513-f390fee58527.png)

- po는 단순히 변수를 출력하는 것에 그치지 않고 다양한 메서드를 실행하여 테스트해볼 수 있음
    
    ![스크린샷 2023-01-05 오후 4 24 22](https://user-images.githubusercontent.com/87598209/211025291-466cda0e-e95b-47f1-a0df-627f895a73e3.png)


- po는 expression의 또 다른 표현임(둘 다 같은 역할을 수행하는 커맨드)
- po 커맨드도 다른 이름으로 customize 해줄 수 있음
    - `command alias '커스텀한 이름' '원래 커맨드 명'`
    
    ![스크린샷 2023-01-05 오후 4 28 56](https://user-images.githubusercontent.com/87598209/211025305-1e37b8d9-5a77-4b72-9532-b132fce63e66.png)

- expression을 my_po로 커스텀해준걸 확인할 수 있다

## “po” Under the Hood

- po를 사용하게되면 내부적으로는 어떻게 동작할까

![스크린샷 2023-01-05 오후 4 32 39](https://user-images.githubusercontent.com/87598209/211025312-d86f16cf-a32e-46cb-bbb2-01f7b2911cde.png)

- 최초 po 커맨드를 실행하면, 컴파일할 수 있는 코드를 만드는 것부터 시작
- 그 후 해당 코드들은 컴파일 되어지기 위해 Swift와 claim Compiler를 거침
- 위의 작업이 완료되면, LLDB는 결과값을 가져오게 되고 이제 사용자가 원하는 object description을 보내기 위한 절차를 수행하게 됨
- LLDB는 description에 접근하기 위한 코드를 생성하고 컴파일함
- 이후 작업이 완료되면 string 타입으로 결과를 가져오게 되고 이제 우리는 console에서 해당 내용을 확인할 수 있음

## “p”

- p도 po와 마찬가지로 콘솔에 변수를 출력해주는 커맨드이다
- 다만 po와 미묘하게 다름

![스크린샷 2023-01-05 오후 5 11 56](https://user-images.githubusercontent.com/87598209/211025332-fe22b349-733e-4193-acaf-4d555dcb064a.png)

- po와 똑같은 정보를 제공하고 있지만 결과 값의 이름에서 po와의 차이가 있음
- $R- 는 LLDB에서의 스페셜한 컨벤션이라고 함
- 해당 데이터로 직접 접근해서 다른 정보들도 확인해줄 수 있음

![스크린샷 2023-01-05 오후 5 25 28](https://user-images.githubusercontent.com/87598209/211025339-b7509695-85b1-45a7-8a2c-b9ee20b843d8.png)

- p도 po와 마찬가지로 expression 커맨드의 alias이나 다만 차이점은 `—object-description` 키워드의 유무이다
    
    ![스크린샷 2023-01-05 오후 5 28 49](https://user-images.githubusercontent.com/87598209/211025357-89efc1c2-86b4-40da-8813-0a6f499c76f1.png)


## “p” Under the Hood

![스크린샷 2023-01-05 오후 5 31 00](https://user-images.githubusercontent.com/87598209/211025370-2cd44356-d280-41e4-90ff-0d1901d03068.png)

- 대체로 po와 내부동작은 비슷하나 결과값을 얻고 난 뒤의 과정에서 조금 차이가 존재함
- po와 달리 Dynamic type resolution이라는 절차를 거치게 됨

### Dynamic type resolution

![스크린샷 2023-01-05 오후 5 50 35](https://user-images.githubusercontent.com/87598209/211025384-282b9d9b-c316-440f-9b21-58d4e5f7b62e.png)

- 위의 코드처럼 Trip 구조체는 Activity라는 프로토콜을 준수하고 있다
- 인스턴스 cruise는 Activity 타입이다
    - cruise는 static 타입인 Activity이지만 런타임 시점에서 Dynamic 타입인 Trip이 됨
- 즉 Activity 타입인 cruise는 소스코드 상에서 name이라는 프로퍼티를 가질 수가 없음(Activity 프로토콜에 name 프로퍼티를 선언하지 않는 한)
    
    ![스크린샷 2023-01-05 오후 6 43 46](https://user-images.githubusercontent.com/87598209/211025407-ea2d26bb-fbb5-4a12-9b90-d6150d1eb1a5.png)

- 따라서 static compiler는 에러를 뱉을 수 밖에 없음
- 굳이 프로퍼티에 접근해야한다면 타입캐스팅을 해줘야 한다
    
    ![스크린샷 2023-01-05 오후 6 45 04](https://user-images.githubusercontent.com/87598209/211025427-43c114a5-dafa-4eb8-9d5a-9ac01452ea16.png)


- Dynamic type resolution 단계를 거치고 나면 마지막으로 사용자가 읽을 수 있는 format으로 전환하는 단계가 남음

![스크린샷 2023-01-05 오후 6 45 32](https://user-images.githubusercontent.com/87598209/211025445-d0fb1236-3e44-4fb3-afef-7bc03aaad372.png)

- 포메팅을 안하면 아래처럼 표현됨
    
    ![스크린샷 2023-01-05 오후 6 47 27](https://user-images.githubusercontent.com/87598209/211025464-6d190a3e-24a1-4a21-9380-815843cba53b.png)

- raw 키워드를 사용하여 formatting 과정을 생략할 수 있음
- formatting을 거치고 나면 다음과 같이 표현된다
    
    ![스크린샷 2023-01-05 오후 6 49 01](https://user-images.githubusercontent.com/87598209/211025480-02c087d8-25c0-407d-8ee7-6158111899ef.png)


## “v”

- v도 p 커맨드와 동일하게 완전 동일한 내용을 보여주고 있음

![스크린샷 2023-01-05 오후 6 49 46](https://user-images.githubusercontent.com/87598209/211025506-6b177733-898c-41ae-baaf-814a8f76a6f0.png)

- v는 p 커맨드와 달리 frame variable 이라는 커맨드의 alias임
    
    ![스크린샷 2023-01-05 오후 6 51 27](https://user-images.githubusercontent.com/87598209/211025518-fa048bce-9353-4438-a5ac-335bb8f8f979.png)


- 또한 p와 po 커맨드와의 가장 큰 차이점은 컴파일 및 실행 과정을 거치지 않음
- 따라서 속도가 매우 빠르다

## “v” Under the Hood

![스크린샷 2023-01-05 오후 7 07 00](https://user-images.githubusercontent.com/87598209/211025535-ed0f0f32-f95f-4f83-a211-f175926c0381.png)

- 컴파일 과정을 거치지 않는 대신 현재 프로그램의 state를 조사해서 메모리로부터 해당 값을 가져온다
- 값을 가져온 뒤 p 커맨드와 마찬가지로 Dynamic type resolution 과정을 거치는데, 하위 필드가 많으면 많을수록 해당 과정을 반복한다
- Dynamic type resolution 단계가 마무리되면 p 커맨드와 동일하게 사용자가 읽을 수 있게끔 formatting 과정을 거친다
- Dynamic type resolution 단계를 여러 번 거친다는 점이 p 커맨드와의 큰 차이점 중 하나이다

다시 p 커맨드에서 실패했던 예시로 돌아오면,,

![스크린샷 2023-01-05 오후 7 13 33](https://user-images.githubusercontent.com/87598209/211025558-02745193-cc8f-4658-b2fc-a73d23ec20b6.png)

- p 커맨드로는 결과값을 가져오는데에 실패했지만, v 커맨드로는 가능한 것을 확인할 수 있음
- **why?** v 커맨드는 Dynamic type resolution 과정을 하위 필드가 있을 때마다 반복해주기 때문 !
- 따라서 v 는 cruise는 Trip의 객체라는 것을 이해할 수 있다
- p 커맨드와 달리 불필요하게 타입 캐스팅을 해줄 필요도 없음
