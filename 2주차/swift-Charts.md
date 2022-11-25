# SwiftUI Charts

- SwiftUI에서 차트를 만들 수 있는 프레임워크 `iOS16+`
- 데이터에 맞는 적절한 시각화를 프레임워크에서 자동으로 제공해주고
- 다크모드, 폰트 크기, 기기 크기와 방향, 손쉬운 사용도 제공함
- Chart는 차트를 보여줄 수 있는 SwiftUI 의 View이고, Chart안에 여러가지 Mark를 통해 차트의 값을 표시하게됨

<img src="https://i.ibb.co/JRtrws6/2022-11-25-1-07-45.png">
<img src="https://i.ibb.co/306T5Y7/2022-11-25-1-40-13.png">

<br>

## Data types
- 데이터 종류 : 양적 데이터, 범주형, 시간 데이터

## Marks
- **차트의 모양** 을 나타내는 요소
- **막대, 점, 선** 등 여러가지 요소를 통해서 나타낼 수 있고, 또한 이 요소들을 **조합**하여 더 보기 쉬운 차트를 만들 수도 있음
- 현재 swift에서는 `BarMark`, `LineMark` ,`PointMark` ,`AreaMark`, `RuleMark`, `RectangleMark` 여섯개를 제공

<img src="https://i.ibb.co/7z3d56Q/2022-11-25-2-29-09.png">
<img src="https://i.ibb.co/Fq9rdVH/image-2.png">

## Properties
- **차트의 속성** 을 나타내는 요소
- **차트의 색상, 심볼의 모양, 크기** 등 여러가지를 설정할 수 있고, **`Mark`** 에 `View Modifier` 처럼 적용할 수 있음

<img src="https://i.ibb.co/qx29WD3/2022-11-25-2-46-08.png" width="50%">

- 위와 같이 제공되는 여러개의 Marks, Properties를 조합해서 아래와 같이 매우 여러 종류의 차트를 만들 수 있음

<img src="https://i.ibb.co/qYQfXJn/2022-11-25-2-05-27.png">

## Example
<img src="https://i.ibb.co/BNSZ45g/2022-11-25-1-48-45.png">

- ForEach 문을 사용해서 여러개의 Mark 생성

```swift
Chart { 
	  ForEach(sampleSales) { sale in
	    BarMark(x: .value("Name", sale.name),
	            y: .value("Sales", sale.sales))
		}
}
```

- **인수가 하나인경우 아래와 같이 사용가능**
```swift
Chart(sampleSales) { sale in
    BarMark(x: .value("Name", sale.name),
            y: .value("Sales", sale.sales))
}
```