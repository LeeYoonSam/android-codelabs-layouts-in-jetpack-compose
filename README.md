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