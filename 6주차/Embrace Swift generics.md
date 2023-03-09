`Generic`은 swift에서 추상 코드를 작성하는 기본 도구로, 코드가 진화할수록 복잡성을 관리하는데 매우 중요함

<br>

`추상화`는 아이디어를 구체적인 세부사항과 구분함
코드에는 `추상화`가 유용하게 쓰이는 여러가지 방법이 있다!

<br>

- 코드를 로컬 변수로 인수분해
<img src="https://i.ibb.co/mTfVZ1t/2022-12-21-8-15-18.png">
동일한 값을 여러번 사용해야하는 경우에 유용

<br>

- 코드를 함수로 인수분해
<img src="https://i.ibb.co/DfxCChB/2022-12-21-8-17-28.png">
동일한 기능을 여러번 사용해야하는 경우에 유용함
<br>
세부 정보가 추상화되며 이 추상화를 사용하는 코드는 세부 정보를 반복하지 않고 현재 일어나고 있는 일에 대한 아이디어를 표현할 수 있음

<br>

- swift에는 concrete type도 추상화 할 수 있음.
<img src="https://i.ibb.co/fDYZCwy/2022-12-21-8-18-25.png">
서로 다른 구현을 가진 동일한 아이디어의 타입 집합이 있는 경우
<br>추상 코드를 작성하여 모든 Concrete Type과 함께 작업할 수 있음.

<br>
<br>

# Session Workflow

1. concrete type을 사용해서 코드를 모델링
2. 공통 기능을 식별
3. 이런 기능을 나타내는 인터페이스를 구축
4. 해당 인터페이스를 사용해 제네릭 코드 작성

<br>

## 우선 농장 시뮬레이션을 위한 코드를 구축해보자!

<br>

<img src="https://i.ibb.co/HxmCr5r/2022-12-21-8-21-47.png">

<br>

## 1. concrete type을 사용해 코드를 모델링
<img src="https://i.ibb.co/bbs5VzS/2022-12-21-8-23-47.png">

- Cow 는 hay(건초)를 먹는 eat 메서드
- Hay 구조체는 건초를 생산하는 작물인 Alfalfa 를 기르기 위한 grow 라는 static 메서드
- Alfalfa 구조체는 Hay 를 수확하는 harvest 메서드
- 마지막으로, 우리는 소에게 먹이를 주는 feed 메서드가 있는 구조체를 추가할 것

이제 농장에서 소에게 먹이를 줄 수 있게 됨

그런데 동물을 더 추가하고 싶어짐

<img src="https://i.ibb.co/5rzPyLF/2022-12-21-8-25-48.png">

먹이는 어떻게 주지?

<img src="https://i.ibb.co/dfz7bpC/2022-12-21-8-28-19.png">

각 매개변수를 개별적으로 받아들이도록 메소드를 오버로드 할 수 있지만
<br>매우 비슷한 구현이 존재하게됨.

또한 동물을 추가하면 할 수록 보일러 플레이트가 됨
<br> → `Generalize` 하라는 신호!

<br>

## 2. 공통 기능을 식별

우리는 모두 특정 유형의 먹이를 먹을 수 있는 동물 타입의 집합을 구축함

각 동물들은 먹는 방법이 다를 거고 따라서 eat 메서드의 각 구현은 행동에서 차이가 있음

우리는 여기서 추상 코드가 eat 메서드를 호출하도록 하고 구체적인 유형에 따라 다르게 동작하도록 구현해야한다.

<br>

다양한 구체적인 타입에 대해 추상 코드가 다르게 동작하게 하는 것을 `다형성`이라고 한다.

`다형성`을 통해 코드 한개가 사용되는 방식에 따라 여러가지 동작을 가질 수 있다.


## Polymorphism
 이렇게 한 코드로 여러 행동을 수행하는 것을 "Polymorphism" 이라고 부름

다형성에는 세 가지 형태가 존재
- 오버로드를 통한 ad-hoc polymorphism
- 서브타입을 이용한 subtype polymorphism
- 제너릭을 이용한 parametric polymorphism

---

1. 함수 오버로드
<br>→ 앞에서 봤던 거!

2. 서브 타입
<br>런타임시 상위 타입에서 코드가 사용하는 특정 하위 타입에 따라 다른 동작을 가짐
→ 클래스 계층 사용
<br>
<img src="https://i.ibb.co/gwcMSyq/2022-12-21-8-32-29.png">
- animal 클래스를 도입해서 각 특정 animal 유형들이 animal 클래스를 상속받도록 하고
eat 메서드를 재정의하도록 함
- animal 클래스에서 eat 메서드를 호출하는 코드는 하위 유형 다형성을 사용해 하위 클래스 구현을 호출하게 됨

## 🚩 red flag
- 아직 animal의 eat 메서드에 대한 매개변수를 입력하지 못함
- 클래스를 사용하면 서로 다른 동물 인스턴스간에 어떤 상태도 공유할 필요가 없거나 공유하기 싫더라도, 참조 의미론을 사용하도록 강요함.
- 또한 이 전략에서는 기본 클래스의 eat 메서드를 하위 클래스가 재정의 하지 않아도, 런타임때까지 탐지되지 않음
- 더 큰 문제는 각 하위 타입의 동물들이 다른 종류의 먹이를 먹는 것임<br>
이 종속성은 클래스 계층으로 표현하기 어려움.

우리가 선택할 수 있는 방법은 메서드가 `Any`과 같은 덜 구체적인 type을 허용하도록 하는 것임

<img src="https://i.ibb.co/qJCL9ZW/2022-12-21-9-21-55.png">

- 그러나 이런 방법은 런타임에 올바른 유형이 전달됐는지 확인하기 위해 하위 클래스의 구현에 의존하게 됨
- 그래서 각각의 재정의된 메서드에 보일러 플레이트를 추가로 부과하게됨
- 우연히 잘못된 음식 타입을 넣으면 버그가 발생하지만, 런타임에 가서야 알 수 있음

<br>

<img src="https://i.ibb.co/BjMvvcn/2022-12-21-9-27-22.png">

- 또 다른 방식으로 접근해보면, Type Parameter(제네릭)는 각 하위클래스에 대한 특정 feed 유형의 플레이스 홀더 역할을 함
- animal이 작동하려면 음식이 필요하긴 하지만, 음식을 먹는게 동물의 핵심 목적은 아님
- 또한 animal과 함께 작동하는 여러 코드들은 음식이 필요하지 않을 수 있음
- animal에 대한 참조는 food type을 지정해야함.
<br>→ 부자연스럽고 불필요한 코드까지 가지게 됨!
- animal 클래스의 각 사용 장소에 있는 이 보일러 플레이트는 각 animal에 고유한 type을 추가하는 경우, 더 부담이 될 수 있음

<img src="https://i.ibb.co/QHZm6qm/2022-12-21-9-32-53.png">

- 근본적인 문제는 클래스가 데이터 유형이라는 거임. 우리는 슈퍼클래스가 구체적인 유형에 대한 추상적인 아이디어를 나타대도록 만들려함
- 대신 우리는 기능의 작동 방식에 대한 세부 정보없이 유형의 기능을 나타내도록 설계된 언어 구조를 원함

동물들에게는 두가지의 공통 기능이 있음

- 특정 음식 타입
- 음식를 섭취하는 작업

<br>

## 3. 이러한 기능을 나타내는 인터페이스를 구축

이제 프로토콜을 사용해서 이런 기능을 나타내는 인터페이스를 구축해보자!

- 프로토콜도 `추상화` 도구로써 적합한 유형의 기능을 설명할 수 있음
- 프로토콜을 사용하여 type이 수행하는 작업에 대한 아이디어를 구현 세부 정보와 분리할 수 있음
- type이 수행하는 작업에 대한 아이디어는 인터페이스를 통해 표현됨
- 동물의 기능을 이제 프로토콜 인터페이스로 변환해보장

<br>

<img src="https://i.ibb.co/5hGv0gn/2022-12-21-9-54-35.png">

- 이제 컴파일러가 프로토콜의 요구사항을 구현하는지 확인하게됨
- associated type 은 구체적인 타입에 대한 placeholder 역할
- 각 동물 유형은 반드시 eat 메서드를 구현해야하고, feed type은 매개변수 목록에 사용되므로 컴파일러는 이 type이 뭔지 유추할 수 있음
- 우리는 성공적으로 동물의 공통기능을 파악하고 프로토콜 인터페이스를 사용해여 기능을 표현했음.

이제 더 제너릭한 코드를 작성할 수 있게 되었음!

<br>

## 4. 해당 인터페이스를 사용해 제네릭 코드 작성해보기
<img src="https://i.ibb.co/30zGgkD/2022-12-22-4-53-08.png">

- 우리는 모든 concrete Animal 타입에 대해 작동하는 하나의 구현을 만들고 싶음
- parametric polymorphism 을 사용하고 메서드가 호출될 때 type parameter 를 도입할 것

<br>

- 프로토콜과 같은 구체적인 타입이 아닌 경우 불투명한 타입(Opaque Type) 이라고 하는데요.
- 불투명한 타입은 파라미터로 받거나, 결과값으로 반환하는 것이 불가능합니다.

<img src="https://i.ibb.co/FhGvzBz/2022-12-21-10-20-59.png">

- 제네릭은 꺽쇠 괄호안의 함수 이름뒤에 작성됨
- 일반 변수 및 함수 매개변수와 마찬가지로 제네릭의 이름도 원하는 대로 지정할 수 있음
- 또한 다른 타입과 마찬가지로 제네릭의 이름을 사용하여 함수 서명 전체에 걸쳐 제네릭을 참조할 수 있음
- A라는 제네릭을 선언하고 A를 동물 기능 매개변수의 타입으로 사용함
- 또한 제네릭이 animal을 준수하도록 함

<img src="https://i.ibb.co/D5M21jB/2022-12-21-10-24-10.png">

- 프로토콜 준수는 where 절로도 표현 가능!
- 이 경우에 메서드가 실제보다 더 복잡해보임. 이런 제네릭 패턴은 너무 일반적이어서
더 쉽게 표현할 수 있는 방법이 있음!!

<br>
<img src="https://i.ibb.co/SxsjVrL/2022-12-21-10-24-22.png">


- 이전 선언과 동일하지만 불필요한 매개변수 목록이 필요없고 where도 사라지게됨
- 따라서 `some` Animal로 작성하는게 더 간단함
- 구문상의 노이즈를 줄여주고 매개변수 선언에 바로 동물 매개변수에 대한 의미 정보를 포함하고 있음

<br>
<img src="https://i.ibb.co/3sTJ1YQ/2022-12-22-5-07-50.png">

- `some` Animal의 `some`은 현재 작업중인 specific type이 있음을 나타냄
- `some` 키워드 뒤에는 항상 제네릭의 제약 조건이 나옴
- 이 경우 특정 유형이 animal 프로토콜을 반드시 준수해야하며, 이를 통해 우리는 매개변수 값에 animal 프로토콜의 요구사항을 사용할 수 있음!
- `some` 키워드는 매개변수 및 결과 유형에 사용할 수 있음

<br>
<img src="https://i.ibb.co/0Z5fqgt/2022-12-21-10-30-15.png">

- 스유에서 나오는 그 `some` 맞음
- result type인 `some` View는 정확히 동일한 개념임
- body 프로퍼티는 `some` view를 반환하지만 body 프로퍼티를 사용하는 코드는 specific type을 알 필요가 없음

## abstract specific type..?
- abstract specific type에 대한 Placeholder를 나타내는 abstract type을 불투명 타입, 즉 Opaque type이라고 함
- 특정 구체적인 유형을 기본 유형 underlying Type이라고 함..


- 간단하게 언더라잉 타입은 리턴 값의 실제 타입 즉, 내부에서 실제로 사용하는 타입을 말함
- Opaque type이 있는 값의 경우 값의 범위에 대해 underlying Type이 고정됨
- 이렇게하면 값을 사용하는 제네릭 코드는 값에 액세스할 때마다 동일한 underlying Type을 얻을 수 있음

<img src="https://i.ibb.co/QPb5j82/2022-12-22-5-23-48.png">

- `some` 키워드와, 꺽쇠 안에 선언된 제네릭을 사용하는 유형은 모두 Opaque type을 선언함

<br>

<img src="https://i.ibb.co/RcCtcGy/image.png">

- Opaque type은 입출력 모두에 사용될 수 있어서, 매개변수 또는 결과 위치에서 선언할 수 있음
- opaque type 의 위치로 프로그램의 어느 부분이 abstract trype 을 보고 concrete type 을 정하는지 결정


- 타입 파라미터는 항상 입력 측에서 선언되므로, 호출하는 부분에서 underlying Type을 결정하고 구현할 때는 abstract type을 사용함

-> 일반적으로 Opaque type 또는 result type에 대한 값을 제공하는 프로그램의 부분이 underlying Type을 결정

-> 이 값을 사용하는 프로그램의 부분은 abstrct type을 봄

<br>

- underlying Type은 `값`에서 추론되므로 항상 값과 동일한 위치에서 나옴
<img src="https://i.ibb.co/3NZ51hT/2022-12-22-12-30-02.png">
- 로컬 변수의 경우, underlying type은 오른쪽에 있는 할당 값에서 추론됨
- 즉 Opaque type의 로컬 변수는 항상 초기값을 가져야함

<br>

<img src="https://i.ibb.co/6WZGfgv/2022-12-22-1-05-56.png">

- 이를 제공하지 않으면 컴파일러에서 오류

<img src="https://i.ibb.co/GdPp9Fp/2022-12-22-5-43-29.png">

- 변수 범위에 대해 underlying type이 고정되어야 하므로, underlying type을 변경하려고 해도 오류가 발생함

<img src="https://i.ibb.co/KF1344W/2022-12-22-5-47-57.png">

- Opaque type 매개 변수의 경우 underlying type은 호출 부분의 인수 값에서 유추됨

<br>

<img src="https://i.ibb.co/9rp2xyB/2022-12-22-5-49-42.png">

### 요 매개변수 위치에 사용할 수 있는 `some` 이 swift 5.7의 새 기능

- underlying type은 파라미터 범위에 내에서만 수정되므로
따라서 각 호출시에 서로 다른 인수 타입을 제공할 수 있음

<br>

<img src="https://i.ibb.co/dc8kGY6/2022-12-22-5-51-40.png">

- Opaque `Result` type의 경우 underlying type은 구현의 `return` 값에서 추론됨
- Opaque `Result` type을 가진 메서드 또는 연산 프로퍼티는 프로그램의 모든 위치에서 호출할 수 있으므로, 이 명명된 값의 범위는 global 전역적임
- 즉 언더 라잉 리턴 타입은 모든 반환 구문에서 `동일해야함`

<img src="https://i.ibb.co/fXRZ60d/2022-12-22-1-28-37.png">

- 그렇지 않으면 컴파일러는 언더라잉 리턴 타입의 유형이 일치하지 않는다는 오류를 보고함

<img src="https://i.ibb.co/1rmM15W/2022-12-22-1-30-26.png">

- 이 경우 ViewBuilder DSL을 통해서 문제를 해결할 수 있음
- 뷰빌더 dsl(?)은 각 분기에 대해 동일한 언더라잉 리턴 타입을 갖도록 제어 흐름 구문을 변환할 수 있음
- @ViewBuilder을 메서드에 작성하고, 반환 구문을 제거하면 ViewBuilder 유형별로 결과를 빌드할 수 있음

<br>

## 🧑‍🌾 Animal의 feed 메서드로 돌아가자..

- 다른곳에서 불투명 타입을 참조할 필요가 없으므로 매개변수 목록에서 some을 사용할 수 있음
- 만약 함수에서 Opaque Type을 여러번 참조해야하는 경우, 타입 매개변수가 유용하게 사용됨

<img src="https://i.ibb.co/yPMvFsH/2022-12-22-6-00-00.png">

- 예를 들어 우리가 animal 프로토콜에 `Habitat` 라는 또 다른 assosiated type을 추가하면 우리는 어떤 동물을 위해 농장에서 서식지를 만들 수 있어야 됨.
- 이 경우 특정 동물 유형에 따라 result type 이 달라지기 때문에 매개변수의 타입과 Return type에 제네릭 변수인 A를 사용해야함

<br>

## 이제 feed 메서드 구현을 구축해보자

<img src="https://i.ibb.co/4VF6vBf/image.png">

- animal 매개변수를 사용하여 grow 하는 작물 타입에 접근
- 다음으로, harvest 메서드를 호출해서 작물을 수확
- 그리고 animal 에게 먹임

기본 동물 유형이 고정되어 있기때문에 컴파일러는 다양한 메서드 호출에 걸쳐서 타입간 관게를 알수있음

이런 정적 관계는 우리가 동물에게 잘못된 종류의 음식을 먹이는 실수를 저지르지 못하게 막아줌

<img src="https://i.ibb.co/7NrzSKM/2022-12-22-1-52-34.png">
보장되지 않는 먹이 유형을 사용하려한다면 컴파일러가 알려줌

<br>
<br>

# 마지막으로 모든 동물에게 먹이를 주는 feedAll 메서드를 추가해보자

<img src="https://i.ibb.co/XSr9Dm2/2022-12-22-1-58-19.png">


<img src="https://i.ibb.co/mSK6SbP/2022-12-22-2-02-46.png">

- 요소 유형이 동물 프로토콜을 따라야한다는 것을 알지만 배열이 다른 동물 유형을 저장하고싶음!

- 기본 유형은 고정되므로 배열의 모든 요소가 동일한 유형을 가져야함
그래서 일부 동물의 배열을 올바르게 표현하지못함
- 그래서 우린 모든 animal 타입을 대표할 수 있는 슈퍼 Type이 필요함

<br>

## any Animal
- `any` Animal는 타입이 임의의 동물 타입을 저장할 수 있으며 기본 동물 타입은 런타임에 달라질 수 있음을 나타냄
- `any` Animal은 모든 구체적인 동물 타입을 동적으로 저장할 수 있는 단일 정적 유형
- 유연한 저장을 위해서 모든 동물 유형은 메모리에 특별한 표현을 가짐


이 표현을 상자처럼 생각할수잇음

<img src="https://i.ibb.co/hXfBBPy/2022-12-22-2-10-25.png">

- 값이 상자 안에 직접 들어갈 정도로 작은 경우도 있지만, 상자보다 더 큰 경우도 있음
- 값을 다른곳에 할당해야하고 상자에는 해당 값에 대한 포인터 값이 저장됨

- `any` Animal과 같은 표현을 Existential type이라고 부름
- 다른 구체적인 유형에 대해 동일한 표현을 사용하는 방법을 type erasure 유형 소거라고 함
- concrete type 은 컴파일 타임에 erase 되고 런타임에만 알려짐


<img src="https://i.ibb.co/h1JHVHb/2022-12-22-2-13-38.png">

- 유형 소거를 사용하여 feedAll 메서드에서 원하는 값 유형의 배열을 작성할 수 잇음

<img src="https://i.ibb.co/vwt5Mq4/image.png">

- 다음과 같이 `associated type`이 있는 프로토콜에 `any` 키워드를 사용하는 것이 Swift 5.7 의 새로운 기능

<img src="https://i.ibb.co/SXdQnSc/image.png">

- 하지만 모든 동물에서 먹기를 호출하려고하면 컴파일로 오류가 발생함
- 특정 animal types에 의존하는 associated types 도 제거해버림.. 그래서 우리는 이 동물이 어떤 먹이를 예상하는지 알 수 없게댐
- !!! 그래서 이 동물이 어떤 유형의 먹이를 기대하는지 알 수 없음

 Animal 에서 직접 eat 메서드를 호출하는 대신 some Animal 을 사용하는 feed 메서드를 호출해야 함

<br>

<img src="https://i.ibb.co/mqM8QfS/2022-12-22-2-26-37.png">

- `any` Animal은 `some` Animal과 다른 타입이지만, 컴파일러는 기본값을 언박싱하고 `some` Animal 매개변수에 이를 직접 전달해서  `any` Animal 인스턴스를 `some` Animal로 변환할 수 있음.

- 이런 언박싱 기능도 swift 5.7의 새로운 기능!
<img src="https://i.ibb.co/Vv1246Z/image.png">




# summary
<img src="https://i.ibb.co/5kyZNX8/image.png">

## `some`
some 을 사용하면 underlying type 이 고정

이를 통해 generic code 의 underlying type 에 대한 type relationships 를 의존할 수 있으므로,

작업중인 프로토콜의 API 및 associated types 에 대한 전체 액세스 권한을 갖게 됨

## `any`
arbitrary concrete type 을 저장하는 경우 any 를 사용

any 는 type erasure 을 제공하여 서로 다른 종류로 이루어진 collections 을 표시

optional 을 사용하여 underlying type 의 부재를 표현

또한 세부적인 구현사항을 추상화할 수 있다!