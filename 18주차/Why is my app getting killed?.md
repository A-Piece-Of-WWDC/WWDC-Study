# Why is my app getting killed?

앱이 백그라운드에서 종료되면, 앱의 진행사항이 저장되지 못하고 데이터가 사라진다.

앱이 백그라운드에서 종료되는 이유들과 

이때 개발자인 우리가 해야할 일에 무엇이 있을까?

# 🚨 Top Causes 6

<img width="293" alt="스크린샷 2023-05-30 오후 7 26 15" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/0550d8c6-431f-41b5-9d0d-f1ed0f24181e">

iOS14 부터는 원인 별로 얼마나 백그라운드 종료가 발생하는 지에 대해 확인할 수 있는 API, MetricKit API를 사용할 수 있다.


![Untitled](https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/3fe0818a-b050-44b4-9164-6b31d39e1793)

## 1. Crashes

<img width="708" alt="스크린샷 2023-05-30 오후 7 28 13" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/3df8ac0f-7f1c-4a47-a6bf-16ff7626ed5b">

### 발생 원인

- 세그먼테이션 오류
    - 읽을 수 없는 메모리 영역에 접근 시 발생
    - ex) 잘못된 포인터, 해제된 메모리 접근, 배열 유효 범위 인덱스 초과하여 접근 등
- illegal 인스트럭션
    - 잘못된 명령어 실행 시 발생
    - ex) 잘못된 CPU 아키텍처, 잘못된 최적화 설정
- Assert 실패 / 예외 미처리

### 추적 방법

- 잘못된 동작의 이벤트들 - crash 로그 발생
- Xcode Organizer 자동으로 표시
- MetricKit - MXCrashDiagnostic

<img width="827" alt="스크린샷 2023-05-31 오전 1 58 53" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/e28dd477-21dc-49f3-9b19-ceadf8f738bb">

## 2. WatchDog

<img width="599" alt="스크린샷 2023-05-30 오전 2 02 49" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/3489b633-03b9-4550-a622-c6511ebdef37">

- 앱의 응답성이 유지되지 않을 때 앱이 종료됨
- 실행, 백그라운드 진입, 포그라운드 진입과 같은 주요 앱 전환
    - 20초의 시간제한이 존재
- 디버거가 연결되어 있는 동안에는 이러한 종료가 발생하지 않음
- 일반적으로 교착상태, 무한 루프, 메인 스레드에서 끝나지 않는 동기 작업과 같은 상황에서 발생 → 심각한 로직 결함 있음을 암시

### 추적 방법

- MetricKit - MXCrashDiagnostic을 통한 진단 보고서로 추적

## 3. CPU 리소스 제한

<img width="694" alt="스크린샷 2023-05-30 오후 7 27 52" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/ae506dbc-bfc1-47f7-9ff1-403ebcde460a">

앱이 백그라운드 실행에 의존하는 경우, 엄격한 CPU 사용 제한이 있다.

백그라운드에서 지속적으로 CPU의 높은 부하가 발생되면, 시스템은 energy exception 리포트를 보내고 이 작업이 지속되면 앱을 종료시킨다.

### 추적 방법

- Xcode Organizer
- MXCPUExceptionDiagnostic 보고서
    - 이 보고서에는 종료시점에 앱이 정확히 무엇을 하고 있었는지에 대한 호출 스택이 포함되어있음
    - 코드에 과도한 CPU 활동을 유발하는 버그가 존재하는지 확인 필요 → 해당 코드가 꼭 필요하다면, background processing task를 통해 디바이스가 충전  중인 동안 CPU 제한 없이 돌게 할 수 있음
    

## 4. Memory footprint exceeded

<img width="696" alt="스크린샷 2023-05-30 오후 7 29 03" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/2b745c38-64f9-4f41-8f77-66ec099bfd77">

CPU와 마찬가지로, 메모리 또한 과도한 사용 시, 앱이 종료된다. 

**메모리 limit** 

- 포그라운드, 백그라운드에서 동일
- 디바이스마다 다르며 오래된 디바이스의 경우 일반적으로 limit이 낮다. (아이폰 6s 이전의 디바이스는 항상 앱이 200MB 이하여야 안전)

### 추적 방법

- Instruments
- Memory Debugger

## 5. Memory pressure exit - jetsam

<img width="680" alt="스크린샷 2023-05-30 오후 7 29 52" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/1726488c-7b8d-4967-83b7-58791283100e">

jetsam은 버그가 아닌, 일반적인 백그라운드 종료 상태로

시스템이 활성화된 foreground 앱을 위해, 백그라운드의 앱을 종료하여 메모리를 확보하는 것

 
<img width="703" alt="스크린샷 2023-05-30 오후 7 30 11" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/16d55cf9-997c-4050-ad9c-9b4d6cca4383">

foreground의 앱의 메모리가 커지게 되면, 우리의 초록앱(background 앱)이 종료된다.

따라서 백그라운드로 이동될 때 메모리 공간을 최대한 작게 하여 종료될 확률을 줄이는 것이 좋다.

애플에서 권고하는 기준은 50MB, 하지만 50MB 이하로 유지해도 jetsam을 완벽히 막을 수는 없다.

따라서 백그라운드로 이동될 때,  UIKit의 Restoration API를 통해 반드시 앱의 상태를 저장해두어야한다.

## 6. Background task timeout

<img width="542" alt="스크린샷 2023-05-30 오후 7 30 41" src="https://github.com/A-Piece-Of-WWDC/WWDC-Study/assets/53509789/19919739-c7dc-4992-a005-f3b0774b96e2">

jetsam 다음으로 일반적인 백그라운드 앱 종료 이유로, 

포그라운드에서 백그라운드로 이동할때, 

- UIApplication.beginBackgroundTask 를 호출하여 중요한 작업이 시작됨을 알리고
- UIApplication.endBackgroundTask를 호출하여 작업이 종료됨음 알린다.
    - 이 함수가 호출되지 않으면 백그라운드 작업이 종료되었다는 것을 시스템은 알지 못하고, timeout되어 앱을 종료 시킨다. (여기서 timeout은 30초만에 발생)

### 추적 방법

- MXBackgroundExitData

### 방지하는 방법

- named variant API (UIKit)
    - 해당 API는 앱의 백그라운드 작업 중, 종료되지 않은 작업을 분리할 수 있음
- expiration handler
