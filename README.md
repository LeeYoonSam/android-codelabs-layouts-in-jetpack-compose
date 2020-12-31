# android-codelabs-layouts-in-jetpack-compose
Codelab - Layouts in Jetpack Compose

https://developer.android.com/codelabs/jetpack-compose-layouts#0

---

## Modifiers

`Modifiers` 는 composable 을 꾸며주는 역할을 한다.

`Modifiers` 는 알반 코틀린 object이다.

`Modifiers` 를 chain 으로 연결하여 구성 가능

`Modifiers`는 순서가 중요하므로 수정자를 연결할 때주의하십시오.
  - 단일 인수로 연결되므로 순서가 최종 결과에 영향을줍니다.
  - 예로 clickable 보다 padding 을 먼저 넣으면 패딩영역은 제외하고 선택이 가능하다.

### 변경 가능한 특성
- 행동
- 외관
- 레이블 접근
- 사용자 입력 처리
- 클릭, 스크롤, 드래그, 줌 등의 인터렉션 추가

### Surface
이미지 로딩중에 `placeholder` 를 보여주려고 할때 사용 할 수 있다.
- 모양과 placeholder 색상 지정
- 크기 지정을 위해 `preferredSize` 를 지정 할 수 있다.

### 기본 특성
- `Modifier.padding` 을 사용해서 `Column` 의 padding 지정
- `@Composable`은 선택적 수정 자 매개 변수를 허용하여보다 유연하게 만들어 호출자가 수정할 수 있도록 합니다.

```kotlin
@Composable
fun PhotographerCard(modifier: Modifier = Modifier) {
    Row(
        modifier
            .padding(8.dp)
            .clip(RoundedCornerShape((4.dp)))
            .background(MaterialTheme.colors.surface)
            .clickable(onClick = { /*TODO*/ })
            .padding(16.dp)
    ) {
        Surface(
            modifier = Modifier.preferredSize(50.dp),
            shape = CircleShape,
            color = MaterialTheme.colors.onSurface.copy(alpha = 0.2f)
        ) {

        }
        Column(
            modifier = Modifier
                .padding(start = 8.dp)
                .align(Alignment.CenterVertically)
        ) {
            androidx.compose.material.Text(text = "Albert Lee", fontWeight = FontWeight.Bold)
            Providers(AmbientContentAlpha provides ContentAlpha.medium) {
                androidx.compose.material.Text(text = "3 minutes ago", style = MaterialTheme.typography.body2)
            }
        }
    }

}

@Preview
@Composable
fun PhotographerCardPreview() {
    AndroidcodelabslayoutsinjetpackcomposeTheme {
        PhotographerCard()
    }
}
```


## Slot APIs

`Compose` 는 UI를 빌드하는 데 사용할 수 있는 `고수준 Material Components composables` 을 제공합니다. 
UI를 만들기 위한 블록이므로 화면에 표시 할 정보를 제공해야합니다.

Slot APIs 는 `Compose` 가 composables 위에 사용자 정의 계층을 가져오기 위해 도입한 패턴입니다.

`Material Button` 을 보면, 버튼의 모양과 내용에 대한 가이드 라인이 있습니다.
이것을 사용하기 위한 간단한 API로 변환 할 수 있습니다.

```kotlin
Button(text = "Button")
```
- 기본적인 버튼 생성


구성요소를 사용자 정의에 맞게 변경하고 싶으면 각각의 엘리먼트를 커스터마이징 할 수 있다.
```kotlin
Button(
    text = "Button",
    icon: Icon? = myIcon,
    textStyle = TextStyle(...),
    spacingBetweenIconAndText = 4.dp,
    ...
)
```
- 버튼의 글자 앞에 아이콘을 배치

예상 할 수없는 방식으로 구성요소를 커스터마이징 하기 위해 여러 매개 변수를 추가하는 대신 슬롯을 추가 했습니다.
`Slots`은 개발자가 원하는대로 채울 수 있도록 UI에 빈 공간을 남깁니다.

예를 들어 버튼의 경우, 아이콘과 텍스트가있는 행을 삽입하려는 사용자가 Button 내부를 채울 수 있습니다.
```kotlin
Button {
    Row {
        MyImage()
        Spacer(4.dp)
        Text("Button")
    }
}

```
- 이를 가능하게 하기위해 하위 구성 가능한 람다 (content : @Composable ()-> Unit)를 취하는 Button 용 API를 제공합니다. 
- 이를 통해 Button 내에서 보여줄 자신의 컴포저블을 정의 할 수 있습니다.


```kotlin
@Composable
fun Button(
    modifier: Modifier = Modifier.None,
    onClick: (() -> Unit)? = null,
    ...
    content: @Composable () -> Unit
}
```
- text 라는 이름의 람다는 마지막 매개 변수입니다.
- 이를 통해 후행 람다 구문을 사용하여 구조화 된 방식으로 Button에 콘텐츠를 삽입 할 수 있습니다.

`Compose`는 `Top App Bar`와 같은 더 복잡한 구성 요소에서 슬롯을 많이 사용합니다.

```kotlin
TopAppBar(
    title = {
        Text(text = "Page title", maxLines = 2)
    },
    navigationIcon = {
        Icon(myNavIcon)
    }
)
```

자신의 `composables`을 빌드 할 때 `Slots API` 패턴을 사용하여 재사용 할 수 있습니다.


## Material Components
Compose는 앱을 만드는 데 사용할 수있는 기본 제공 Material Component composables과 함께 제공됩니다.
가장 높은 수준의 composables은 Scaffold입니다.

### Scaffold
`Scaffold`를 사용하면 기본 머티리얼 디자인 레이아웃 구조로 UI를 구현할 수 있습니다.
TopAppBar, BottomAppBar, FloatingActionButton 및 Drawer와 같은 가장 일반적인 최상위 Material 구성 요소에 대한 슬롯을 제공합니다.
Scaffold를 사용하면 이러한 구성 요소가 올바르게 배치되고 함께 작동하는지 확인합니다.


코드를 재사용하고 테스트 할 수 있도록하려면 코드를 작은 덩어리로 구성해야합니다.
이를 위해 화면 `content` 로 또 다른 구성 가능한 함수를 만들어야 한다.
```kotlin
@Composable
fun LayoutsCodelab() {
    Scaffold { innerPadding ->
        BodyContent(Modifier.padding(innerPadding))
    }
}

@Composable
fun BodyContent(modifier: Modifier = Modifier) {
    Column(modifier = modifier) {
        Text(text = "Hi there!")
        Text(text = "Thanks for going through the Layouts codelab")
    }
}
```

### TopAppBar
`Scaffold`에는 @Composable () (()-> Unit)? 유형의 topBar 매개 변수가있는 최상위 AppBar 용 슬롯이 있습니다. 
즉, 원하는 `composable`로 슬롯을 채울 수 있습니다.
```kotlin
@Composable
fun LayoutsCodelab() {
    Scaffold(
        topBar = {
            Text(
                text = "LayoutsCodelab",
                style = MaterialTheme.typography.h3
            )
        }
    ) { innerPadding ->
        BodyContent(Modifier.padding(innerPadding))
    }
}
```

대부분의 머티리얼 컴포넌트의 경우 `Compose`에는 제목, 탐색 아이콘 및 작업을위한 슬롯이있는 TopAppBar 컴포저블이 함께 제공됩니다.
또한 각 구성 요소에 사용할 색상과 같이 재질 사양에서 권장하는 사항에 맞게 조정되는 몇 가지 기본값이 함께 제공됩니다.

`Slots API` 패턴에 따라 `TopAppBar`의 제목 슬롯에 화면 제목이있는 텍스트가 포함되기를 원합니다.


### Placing modifiers
새로운 `composables`을 만들 때마다 `Modifier`로 기본 설정되는 `modifier` 매개 변수를 사용하는 것은 `composables`을 재사용 할 수 있도록 만드는 좋은 방법입니다.
`BodyContent` 컴포저블은 이미 매개 변수로 `modifier`를 사용합니다. BodyContent에 패딩을 더 추가하려면 패딩 수정자를 어디에 배치해야 할까요?
1. BodyContent에 대한 모든 호출이 추가 패딩을 적용하도록 컴포저블 내부의 유일한 직접 자식에 `modifier`를 적용합니다.
```kotlin
@Composable
fun BodyContent(modifier: Modifier = Modifier) {
    Column(modifier = modifier.padding(8.dp)) {
        Text(text = "Hi there!")
        Text(text = "Thanks for going through the Layouts codelab")
    }
}
```

2. 추가 패딩이 필요할 때 추가 패딩을 추가하는 컴포저블을 호출 할 때 `modifier`를 적용합니다.
```kotlin
@Composable
fun LayoutsCodelab() {
    Scaffold(...) { innerPadding ->
        BodyContent(Modifier.padding(innerPadding).padding(8.dp))
    }
}
```

### More icons
[Material icons](https://material.io/resources/icons/?style=baseline)

`ui-material-icons-extended` dependency 를 추가하면 더 많은 아이콘을 사용할 수 있다.
```
dependencies {
  ...
  implementation "androidx.compose.material:material-icons-extended:$compose_version"
}
```

### Further work
`Scaffold`와 `TopAppBar`는 머티리얼처럼 보이는 애플리케이션을 만드는데 사용할 수 있는 컴포저블입니다.
`BottomNavigation` 또는 `Drawer`와 같은 다른 구성 요소에 대해서도 동일하게 수행 할 수 있습니다.