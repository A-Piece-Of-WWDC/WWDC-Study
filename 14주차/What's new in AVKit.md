# ****What's new in AVKit****

- WWDC 2021에서 소개된 새로운 AVKit 기능에 대한 세션
- Picture in Picture(PIP) 기능 향상에 대한 소개

## Picture in Picture(PIP)란?

- 한 번쯤 흔히들 써봤을 기능으로, 우리가 어떤 영상을 보다가 메시지가 와서 홈화면으로 이동하거나 또 다른 행동들을 하고 싶을때 영상 감상은 계속 유지하면서 다른 행동들도 할 수 있게끔 만들어주는 기능

![스크린샷 2023-03-15 오후 8 16 43](https://user-images.githubusercontent.com/87598209/229332444-6968538e-2c39-4907-82a6-ae4864e94ce8.png)

- 이런 식으로 메시지에 대해 답장을 하면서, 영상도 계속해서 볼 수 있다 !

## New property - canStartPictureInPictureAutomaticallyFromInline

- WWDC 2021에서 PIP관련 새롭게 추가된 프로퍼티는 canStartPictureInPictureAutomaticallyFromInline이다.
    
    ![스크린샷 2023-03-15 오후 8 20 42](https://user-images.githubusercontent.com/87598209/229332447-8317a9ea-2358-4648-a97d-dc8975dcb718.png)

- iOS 14.2 이상에서 사용 가능한 프로퍼티로 우리가 영상을 보던 중에 홈화면으로 스와이프해서 이동하게 됐을 때, 자동으로 PIP를 실행시켜줄 수 있게 만들어주는 프로퍼티.
- 이 프로퍼티는 네이티브 컨트롤 기능을 사용하는 AVPlayerViewController, 따로 커스텀한 컨트롤 UI를 사용하는 AVPictureInPictureController 둘 다 사용할 수 있음

## Support PIP - AVSampleBufferDisplayLayer

- 기존에는 AVPlayerLayer의 경우에만, PIP 기능을 지원해주고 있었음. 그러나 이제 iOS 15.0부터는 AVSampleBufferDisplayeLayer에서도 PIP 기능을 지원한다
    
    ### AVSampleBufferDisplayLayer?
    
    - 기본적으로 큰 개념은 AVPlayerLayer와 동일하게 비디오 재생을 위해 사용되는 AVFoundation 프레임워크의 클래스 중 하나.
    - AVPlayerLayer와의 가장 큰 차이점은 AVSampleBufferDisplayLayer는 비디오 샘플 데이터를 미리 메모리에 로드하고 디코딩하는 방식. 따라서 속도가 훨씬 빠르다는 장점이 있음.
    - 따라서, 비디오의 성능이 매우 중요한 앱의 경우, AVPlayerLayer보다는 AVSampleBufferDisplayLayer를 사용하는 것이 유용할 수 있다.
    - 다만, UIKit의 뷰 와의 통합성은 상대적으로 AVPlayerLayer보다 떨어지기에, 복합적으로 다른 뷰와의 인터랙션이나 복잡한 애니메이션이 들어가야될 경우에는 AVPlayerLayer를 쓰는 것이 좋음

- AVSampleBufferDisplayLayer에 PIP를 적용하는 법은 매우 간단함. displayLayer를 지정해주고 playbackDelegate를 지정해주면 된다.
    
    ![스크린샷 2023-03-15 오후 8 39 27](https://user-images.githubusercontent.com/87598209/229332449-cd95164c-6c8b-41c6-85e4-e3022bcc642d.png)

    ![스크린샷 2023-03-15 오후 8 43 56](https://user-images.githubusercontent.com/87598209/229332454-a0911eba-ea8d-432e-961f-073aeac433ee.png)

- 위의 프로토콜을 채택해주고 우리는 그에 맞는 PIP 액션을 처리해준다.
- 총 5가지의 메서드가 존재.
    
    ![스크린샷 2023-03-15 오후 8 44 59](https://user-images.githubusercontent.com/87598209/229332460-c823b6ff-014e-43f4-8c11-9fda06ae18b9.png)

- 첫 번째로, setPlaying의 경우 유저가 PIP 모드에서 Play/Pause 버튼을 클릭할 시 호출된다.
- 두 번째로, skipByInterval의 경우 유저가 forward 버튼이나 backward버튼을 클릭할 시 호출된다.
    
    ![스크린샷 2023-03-15 오후 8 46 41](https://user-images.githubusercontent.com/87598209/229332461-7782cb1a-2b41-41d1-bd43-1291bdb24a65.png)

- 세 번째로, timeRangeForPlayback의 경우, 현재 콘텐츠의 재생 가능한 시간 범위를 반환해준다
    
    ![스크린샷 2023-03-15 오후 8 51 24](https://user-images.githubusercontent.com/87598209/229332467-dcf16183-c5c5-44a3-b04c-34819a067905.png)

- 네 번째로, didTransitionToRenderSize의 경우 유저가 핀치 투 줌 (pinch-to-zoom)과 같이 PIP의 크기를 변경할 때 호출된다.
    
    ![스크린샷 2023-03-15 오후 8 52 44](https://user-images.githubusercontent.com/87598209/229332469-e19662e1-9f68-4525-a2be-95d25448f590.png)

- 마지막으로 , isPlaybackPaused의 경우 주기적으로 호출되며, 일시 중지 또는 재생 상태를 PiP UI에 전달하기 위해 사용된다.
