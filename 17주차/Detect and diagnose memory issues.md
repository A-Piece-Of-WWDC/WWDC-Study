## 애플리케이션의 메모리 사용량이 미치는 영향

왜 우리는 메모리 공간에 관심을 가져야할까?

핵심은, 사용자 경험 향상이다.

<img width="1324" alt="스크린샷 2023-05-02 오후 5 22 38" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/75fd7387-a4a5-4fbd-aa97-888f77560154">

- 앱이 background에 있을 때, 시스템이 앱을 종료시키는 것 방지 → 앱의 활성화 속도 증진
- 메모리 회수를 위한 대기 비용 절약 → 반응 속도 향상
- 복잡한 기능 (동영상,애니메이션) 추가 편의
- 구형 장치에서도 동일한 성능 발휘 가능


### 메모리 종류

<img width="1208" alt="스크린샷 2023-05-02 오후 5 51 09" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/f667752b-8426-447c-a09c-d5c4b73a418d">

- Dirty 메모리: 앱에서 사용 중인 메모리 중 수정된 적이 있는 메모리
    - 힙 할당, 디코딩된 이미지 버퍼, 프레임워크 사용 영역 등
- Compressed 메모리: 최근 액세스 되지 않은 Dirty 메모리 중, 압축된 형태로 저장되어 있는 메모리
    - 액세스 시 압축 해제됨
- Clean 메모리: 사용되지 않은 메모리
    - 메모리 매핑된 파일, 프레임워크 등

앱이 실행 중인 동안 사용하는 메모리  ⇒ Dirty + Compressed 메모리 (clean 메모리는 포함되지 않음.)
  
  
## 메모리 사용량을 프로파일링 하는 도구

Xcode는 개발 및 작업 플로우 전체에서 앱의 메모리 성능을 모니터링하는 도구 XCTest, MetricKit, Xcode Organizer 들을 제공한다.

<img width="1409" alt="스크린샷 2023-05-02 오후 6 18 05" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/b4c61ace-e264-45b0-a769-37efde24df8b">
  
  
### **XCTest 프레임워크**

- 메모리 사용량, CPU 사용량, 디스크 쓰기, Hitch rate, Wall clock time, application launch 시간
    - `measure(metrics:options:block:)` API를 사용하여 메모리 사용량 측정

<img width="450" alt="스크린샷 2023-05-02 오후 6 50 13" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/fbc50bb4-ee6e-4895-918e-8d043f71d083"> <img width="450" alt="스크린샷 2023-05-02 오후 6 50 37" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/13f0645a-cfae-4bdf-80a2-8e51353168fb">

> 막대 그래프는 각각의 반복 측정값을 보여주고, 반복 측정값의 평균을 계산하여 향후 테스트의 기준값으로 설정할 수 있다. 기준값을 설정한 후에는, 기준보다 높은 평균 값이 나오면 테스트가 실패하게 된다. 이러한 차이가 발생하고 실패하는 것을 "회귀(regression)"라고 부른다. 회귀가 발생하면 코드를 조사하고 수정하여 테스트를 통과하도록 만들어야 한다.
> 

<img width="1427" alt="스크린샷 2023-05-02 오후 6 55 39" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/959b11ea-f1ad-492e-a0c0-e65357be93b3">

- Xcode 13부터 성능 테스트의 regression 분석을 위한 기능 추가
    - ktrace 파일
        - 렌더링 파이프라인이나 메인 스레드 차단과 같은 구체적인 문제를 조사하는 데 유용
    - 메모리 그래프

→ 이를 활성화하려면 enablePerformanceTestsDiagnostics 플래그 설정

<img width="398" alt="스크린샷 2023-05-02 오후 8 22 45" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/6e5b422d-1fba-4c9d-9c9f-29aceb0dc228">

- 이전에 작성한 성능 테스트가 완료되면, 콘솔에는 테스트 통과 여부와 그에 대한 이유가 나타남.
    
<img width="1372" alt="스크린샷 2023-05-02 오후 8 26 53" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/d27393ba-4567-4c85-8d5e-f260a20188bb">

- 가장 아래 xcresult 번들 경로가 나타남
    - Xcode에서 xcresult 번들을 열면 테스트 이름 옆에 메모리 측정치를 확인 가능
        
        <img width="480" alt="스크린샷 2023-05-02 오후 8 28 22" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/f5d34503-777a-4dff-bbf6-9d62699a1cf9">
        
        <img width="690" alt="스크린샷 2023-05-02 오후 8 29 08" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/6ac9efe6-15c2-47b4-a00e-ef24d1d79d4e">

    - 또한 테스트 로그를 확장하고 아래쪽으로 내려가면 메모리 그래프 첨부 확인 가능
    - 측정 반복 시작시점에는 pre 접두어가 붙은 초기 메모리 그래프를 수집하고, 반복 끝시점에는 post 접두어가 붙은 두 번째 메모리 그래프를 수집.
  
  
## 발생할 수 있는 메모리 유형

메모리 그래프에서 나타날 수 있는 두 가지 메 모리 이슈
  
  
### 누수(leaks)

- 대부분의 Swift 객체는 Swift의 자동 참조 카운팅 시스템(ARC)로 관리되므로, 대부분의 누수는 방지된다. 그러나 ARC로 관리되지 않는 객체(unsafe pointer 등)를 다룰 때는 참조를 잃기 전에 먼저 해제해야 한다. 또한, ARC로 관리되는 객체도 강한 순환 참조의 일부가 될 수 있으므로, 순환 참조가 필요한 경우 약한 참조를 고려해야한다.
- 메모리 그래프
    - 메모리 그래프 출력된 결과를 통해 총 240바이트의 4개 누수가 있음을 확인
    - retain cycle이 발생했음을 알 수 있고 meal plan과 menu item 객체를 포함하고 있는 것을 확인할 수 있다.
    
    <img width="1154" alt="스크린샷 2023-05-02 오후 7 46 27" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/97ce3d10-f366-4a4b-9e40-c81b55c1352d">
    
    <img width="1680" alt="스크린샷 2023-05-02 오후 7 47 48" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/b5b2a333-d479-43ca-8e04-7b36e91605f5">

  
  
### 힙(heap)

- **할당 회귀(allocation regressions)**
    - 힙은 단순히 동적으로 할당된 개체가 저장되는 프로세스 주소 공간의 한 섹션. 
    힙 할당 회귀는 프로세스가 이전보다 더 많은 객체를 힙에 할당하여 메모리 사용량이 증가하는 것.
    - 따라서 이를 방지하기 위해, 사용하지 않는 할당을 제거하고 불필요하게 큰 할당을 줄여야함.
    - vmmap 툴을 이용하여 메모리 확인
        
        <img width="780" alt="스크린샷 2023-05-02 오후 8 08 38" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/ede825e8-74cc-48d7-8448-ead0093a9708">

    - pre memgraph랑 post memgraph를 비교
    - -summary를 사용하여 메모리 사용 상황을 확인
        
        <img width="991" alt="스크린샷 2023-05-02 오후 8 10 10" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/503a457d-3891-44e3-b005-cfd85f502237">

        - MALLOC_ 영역 = 내 힙 객체가 모두 포함
        - swapped = compressed 메모리
  
  
- **파편화(fragmentation)**
    - 메모리 할당이 반복되면서 사용되지 않은 작은 공간들이 떨어져서 프로그램의 힙 영역이 조각나는 상황
        
        <img width="1391" alt="스크린샷 2023-05-02 오후 8 46 08" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/03923e0d-b7e4-47f3-9942-c66881d6a57b">

    - 시스템이 프로세스에 부여하는 최소 메모리 단위를 페이지라고 하는데, 이 페이지라는 단위는 나눌 수 없기 때문에 페이지의 일부가 쓰여도 dirty 페이지로 간주 → 100% 사용되지 않는 dirty 페이지 생김
        
        <img width="1434" alt="스크린샷 2023-05-02 오후 8 47 39" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/b98dd474-060a-4caa-b1d2-a71a497f3f45">
        
        <img width="1553" alt="스크린샷 2023-05-02 오후 8 46 31" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/0069e843-6519-4ab1-a31b-51fd4aa0211b">
        
        <img width="1418" alt="스크린샷 2023-05-02 오후 8 47 45" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/b7f49419-f79f-4090-ae31-ab9ab1d555b5">
        
        <img width="1573" alt="스크린샷 2023-05-02 오후 8 46 44" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/4adb783a-0e13-49bb-8795-a7325f7d9d85">


    - 따라서 이러한 파편화를 방지시키기 위해서는, 유사한 라이프타임을 가진 객체끼리 할당하는 것이 가장 좋은 방법
    - 현실에서는 fragmentation이 불가피함 → 줄이는 것에 목표를 두는 것이 좋음
    - 파편화 줄이는 방법
        - autorelease pool 사용
            - 범위를 벗어나는 개체는 즉시 해제 → 이 개체들의 수명 비슷해짐.
        - DIRTY+SWAP FRAG SIZE를 보면 낭비되는 공간 확인 가능
            - 권장은 25% 전후
        
        <img width="1470" alt="스크린샷 2023-05-02 오후 8 49 47" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/e78c199d-fb7e-4d9e-80cf-55299538675b">
        
        - instrument의 allocations를 통해 어떤 개체가 유지되고 소멸되는지 확인
            - 파편화 맥락에서 지속되는 개체가 페이지를 dirty하게 만드는 개체
        
        <img width="746" alt="스크린샷 2023-05-02 오후 8 51 21" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/73e48e96-623d-4abb-a511-8efdb36e4d08">
        
        
