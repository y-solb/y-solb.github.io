---
layout: post
title: "[CSS] Flexible box"
date: 2021-01-05 20:30:28 +0530
categories: CSS
---

예전에는 position, float, table을 이용해서 레이아웃을 짰지만 복잡하고 많은 시간이 소요됨

100vh(view height를 100%로 쓰겠다)

# Container

---

display/flex-direction/flex-wrap/flex-flow/justify-content/align-items/align-content

중심축(main axis)와 반대축(cross axis)

### display

너 이제부터 flexbox야!

-   flex : 수직방향 쌓기(block)
-   inline-flex : 수평방향 쌓기(inline)

### flex-direction

-   row : 수평 중심축 (왼쪽 ~> 오른쪽)
-   row-reverse : (오른쪽 ~> 왼쪽)
-   column : 수직 중심축 (위 ~> 아래)
-   column-reverse : (아래 ~> 위)

### flex-wrap

item이 한줄에 꽉차게 되면 다음 줄로 넘어가게 해준다

-   nowrap : wrap하지 않음
-   wrap : wrap되어 한줄에 꽉차면 다음줄로 넘어간다.
-   wrap-reverse : 순서가 반대로 wraping

### flex-flow

-   한줄에 표현이 가능하다

```css
flex-flow: column nowrap;
```

수직 방향으로 nowrap하겠다

### justify-content

중심축에서 item을 어떻게 배치할것인지 (main axis)

-   flex-start : 왼쪽으로 배치(순서는 유지)
-   flex-end : 오른쪽으로 배치
-   center : 중앙 배치
-   space-around : item을 둘러싼 space를 만들어줌
-   space-evenly : 똑같은 간격으로 둘러쌈
-   space-between : 맨 왼쪽과 오른쪽은 딱 맞게 배치하고 중간에 space

### align-items

-   baseline : text가 균일하게 보여지도록

### align-content

-   space-between : 맨 위와 아래는 딱 맞게 배치하고 중간에 space
-   center : 중앙 배치

# Item

---

order/flex-grow/flex-shrink/flex-basis/align-self

### order

-   숫자 : 위치를 지정할 수 있음

### flex-grow

컨테이너를 채울때 얼마나 커질지(비율로 지정)

### flex-shrink

컨테이너가 작아졌을때 얼마나 작아질지(비율로 지정)

### flex-basis

item들이 공간을 얼마나 차지하는 세부적으로 명시

-   auto : flex-grow와 flex-shrink에 따라 변형
-   숫자% : %에 따라서 비율이 지정

### align-self

아이템 별로 아이템을 정렬할 수 있음
