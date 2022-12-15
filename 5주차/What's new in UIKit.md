앱을 관통하는 강력한 프레임워크인 UIKit!

생산성을 위한 UI개선과
컨트롤 개선사항,
API 개선사항,
UIKit와 SwiftUI를 함께 사용할 수 있는 방법에 관해 이야기함

<br>

# Productivity
- 개선된 navigation bar와 새로운 title menu의 기능
- find and replace
- cut, copy, paste 등 편집 인터렉션 개선


<br>

## 1. navigation bar
<br>
<img src="https://i.ibb.co/7k3tDYm/2022-12-15-6-56-55.png">

데스크톱 수준의 툴바 기능을 지원하기 위해 업데이트됨

iOS 16의 UIKit에서는 2개의 내비게이션 방식이 추가.

- Browser : 웹과 문서 브라우저처럼 이력이나 폴더 구조를 이용해서 탐색하기 적합
- Editor : 문서 편집에 적합

<br>

<br>

### Center item
iOS16에서는 앱에 bar button items 을 추가할 수 있음. 여기에 속한 버튼은 내비게이션 바 중앙에 배치됨

<img src="https://i.ibb.co/VQtd8wk/2022-12-15-7-05-32.png">

메뉴에서 도구모음 커스텀 편집을 탭하면

<img src="https://i.ibb.co/xfNGRnn/2022-12-15-7-05-40.png">
<img src="https://i.ibb.co/5vCLstw/2022-12-15-7-06-53.png">

팝업창에서 항목을 드래그해서 재배치할 수 있음

<img src="https://i.ibb.co/L8hkmSq/2022-12-15-7-08-05.png">

만약 다른 앱을 함께 실행해서 크기를 맞춰야하는 상황이 발생하면 시스템에서 자동으로 overflow menu를 제공해줌

<br>

## 2. Title menu
<br>
새로운 내비게이션 스타일에 맞는 타이틀 메뉴를 추가해서 몇가지 기능을 지원함

- duplicate
- move
- rename
- export
- print

delegate 메서드를 적용해서 구현할 수 있음

<img src="https://i.ibb.co/MDyLyhx/2022-12-15-7-12-16.png" >
타이틀 메뉴에 커스텀 항목을 추가할 수도 있음

<br>

### Mac Catalyst

Mac Catalyst로 빌드하면 개선된 내비게이션 바를 이용할 수 있음

NSToolbar랑 매끄럽게 통합되며 추가 코드도 필요하지 않음

<img src="https://i.ibb.co/YXSLF0w/image.png" alt="image">

<br>

## 3. Find and replace
iOS16는 여러 앱에 걸쳐 텍스트를 편집하는 방법을 도입함

첫번째는 새롭게 바뀐 find and replace임

<img src="https://i.ibb.co/b1Wbwy2/image.png">

텍스트에 맞게 설계된 search

UITextView나 WKWebView와 같은 내장된 UIKit뷰에서 플래그를 설정하면 이 기능을 활성화할 수 있음

<br>

## 4. Editing interactions
대규모 업그레이드된 edit Menu

이제 input method 에 따라서 edit menu가 다르게 보임

touch interaction에서는 훨씬 상호작용적으로 재디자인된 menu 를 갖게 됨

<img src="https://i.ibb.co/qN5sXzv/image.png">

포인터를 사용할 때는 모든 기능이 들어있는 컨텍스트 메뉴가 나옴

<img src="https://i.ibb.co/sKY51K6/image.png">

textView의 메뉴에 actions를 삽입하는 새로운 API도 생김

<br>

## Addtionally

iOS16에서는 silde over 모드에서 추가적인 코드 없이도 사이드바 활성화가 가능함

<img src="https://i.ibb.co/cTpRyQm/image.png">


<br>

# Control enhancements
## 1. UICalendarView가 나옴!

다양한 선택 동작을 지원하는데,
선택 단일 날짜나 여러 날짜의 선택이 가능

사용 가능한 날짜 범위 설정은 물론 선택 범위에서 일부 날짜를 제외할 수도 있음

각 날짜를 장식할 수 있음

<img src="https://i.ibb.co/fqbnKQV/image.png">


UIDatePicker랑 다른 점은 캘린더뷰는 날짜를 표현할 때 NSDate가 아닌 NSDateComponent를 사용한다는 점

NSData는 시간의 한 지점을 나타낸다면, NSDataComponents는  특정한 날짜를 더 정확하게 대표함.

어떤 캘린더를 사용해야하는지 지정해야댐


<a href="https://ibb.co/WxPZwz7"><img src="https://i.ibb.co/2nsxQMR/2022-12-15-7-20-48.png" alt="2022-12-15-7-20-48" border="0"></a>

- Calendar view 를 생성하고 delegate 를 설정 
- 캘린더를 Gregorian NScalendar 로 정하기 위해서
calendarView 의 calendar 프로퍼티를 설정

- multi-date 설정하기 위해서 UICalendarSelectionMultiDate 객체를 만들고, 

- selectedDates 프로퍼티를 여러분의 data model 에 있는 기존 날짜에 설정하여(=myDatabase.selectedDates()) 캘린더 뷰에 나타나게 할 수 있음

- 캘린더의 개별 날짜의 선택을 막기 위해서 calendar 의 selection’s delegate 로 부터 multiDateSelection:canSelectDate: 메서드를 적용하여 선택할 수 있는 날짜를 제어

- 선택할 수 없는 날짜는 calendar view 에서 회색으로 표시

- (hasAvailabilities(for:) 메서드는 DateComponents 매개변수가 선택할 수 있는지 제어)

### Configure Decorations
<img src="https://i.ibb.co/XC3sjSh/2022-12-15-7-22-01.png">

개별 날짜에 장식하고 싶다면 calendar delegate 에서 calendarView:decorationForDateComponents: 를 적용


## 2. UIPageControl
현재 페이지에 커스텀 indicator 이미지를 추가하여 페이지의 선택 여부에 따른 다른 이미지를 선택할 수 있음

이제 page control 의 direction(이동방향) 이나 orientation(고정된 상황에서의 방위)을 완벽하게 커스텀할 수 있게됨

<img src="https://i.ibb.co/Yh7H3nv/image.png">


## 3. UIPasteControl
iOS 15에서는 앱이 프로그래밍적으로 system 이 제공하는 Paste interfasces 를 통하지 않고 pasteboard 에 접근하면 pasteboard 에 접근했다는 배너가 나타났음

<img src="https://i.ibb.co/Jvb3mQk/2022-12-15-8-01-16.png">

iOS 16 에서는 배너 대신 pasteboard 의 사용 허가를 묻는 알림창

<img src="https://i.ibb.co/brPWrWB/2022-12-15-8-01-18.png">

사용자가 system 의 paste interface 를 사용하면 pasteboard 에 대한 암묵적인 액세스를 제공하여 alert 창말고 배너 등장

-> paste interface : edit menu 등에서 copy

-> pasteboard : 제스처(손가락 세 개 모으기)를 통해서 복사

# API refinements
iOS15에서는 sheet에 detent를 추가하여 유연하고 역동적이 UI를 구축할 수 있었음

## 1. Customizing Sheets
iOS16에서는 커스텀 detent를 지원하여 sheet의 크기를 마음대로 조절할 수 있음

<img src="https://i.ibb.co/gVfLwxt/image.png">

<img src="https://i.ibb.co/QDk5gZJ/image.png">

<img src="https://i.ibb.co/xmPksSq/image.png">

## 2. SF Symbool
SF Symbol 에서도 새로운 기능이 있음
sybol 은 4가지 랜더링 모드를 지원

<img src="https://i.ibb.co/8Nv52Qq/2022-12-15-8-14-55.png">

<img src="https://i.ibb.co/846DRvG/2022-12-15-8-15-48.png">

iOS 15에서는 렌더링 모드를 지정하지 않았을 때 모노크롬 렌더링을 사용
iOS 16에서는 이런 symbol 들의 기본값은 hierarchical

```swift
UIImage.SymbolConfiguration.preferringMonochorme()
```
명시적으로 모노크롬을 사용하고 싶다면 이렇게

<br>

<img src="https://i.ibb.co/wgVDw7g/image.png">
UIKit 은 variable symbol 지원하면서 0과 1사이의 값에 따라 심벌의 변화를 나타낼 수 있음

현재 볼륨의 레벨을 심벌로 표현할 때, 앱은 speaker.3.wave.fill 심벌을 사용할 수 있는데 variable rendering 을 지원하도록 업데이트됨

<img src="https://i.ibb.co/cXsMpHF/image.png">

<br>

## 3. Swift Concurrency and Sendable
UIKit 을 새로운 Swift concurrency 기능에 맞춰서 개선

UIImage 와 UIColor 와 같은 immutable(불변) types 을 Sendable 을 준수하는 것을 포함하여 MainActor 와 custom Actor 사이에서 컴파일러의 경고 없이 전송이 가능함

<img src="https://i.ibb.co/gd1qRyK/image.png">


<br>

## 3. Stage Manager

이제 더이상 앱에 main screen 이 있다고 가정하지 않아도 됨...

대신 trait collection 과 UIScene API 들과 같은 구체적은 API 들을 따르며 원하는 정보를 얻을 수 있음...

<img src="https://i.ibb.co/0mmzfxH/2022-12-15-8-27-16.png">

....

<br>


## 4. Self-resizing cells

- UICollectionView 와 UITableView 의 self resizing 크게 업데이트 됨

- iOS 16 에서는 cell 의 콘텐츠가 변경될 때 자동으로 새로운 콘텐츠에 맞춰서 셀 크기를 조정하게 됨

- seflSizingInvalidation 프로퍼티가 추가되어 이 새로운 기능을 제어

- selfSizingInvalidation 이 활성화되어 있을 때 셀은 포함된 collection 또는 table view 통하여 크기 조정을 요청할 수 있음


# UIKit and SwiftUI

- UIHostingConfiguration을 통해서
SwiftUI 를 사용하여 collection 과 table veiw 를 위한 새로운 방식으로 셀을 만들 수 있음

<img src="https://i.ibb.co/cQXYfhH/image.png">
SwiftUI로 커스텀셀 만들기..

<br>


# A Couple of Smal but Important Changes
- 사용자가 추적되는 것을 방지하기 위해서 UIDevice.name 은 이제 유저의 커스텀 기기 이름이 아닌 모델 이름을 반환
- 커스텀된 이름을 사용하기 위해서는 이제 entitlement(권한) 를 요구
- UIDevice.orientation 설정은 이제 지원하지 않음
- UIViewController API 인 preferredInterfaceOrientation 으로 인터페이스의 의도된 방향을 지정할 수 있음