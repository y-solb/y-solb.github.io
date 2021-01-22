---
layout: post
title: "Flexible box"
date: 2021-01-05 20:30:28 +0530
categories: CSS
---

예전에는 position, float, table을 이용해서 레이아웃을 짰지만 복잡하고 많은 시간이 소요됨

100vh(view height를 100%로 쓰겠다)

# Container

---

display/flex-direction/flex-wrap/flex-flow/justify-content/align-content/align-items

주축(main axis)과 교차축(cross axis)

### display

너 이제부터 flexbox야!

- flex : 수직방향 쌓기(block)
- inline-flex : 수평방향 쌓기(inline)

### flex-direction

items을 주축으로 설정한다.

- row : 수평 중심축 (왼쪽 -> 오른쪽)
- row-reverse : (오른쪽 -> 왼쪽)
- column : 수직 중심축 (위 -> 아래)
- column-reverse : (아래 -> 위)

### flex-wrap

item이 한줄에 꽉차게 되면 다음 줄로 넘어가게 해준다

- nowrap : wrap하지 않음 (한 줄에 나옴)
- wrap : wrap되어 한줄에 꽉차면 다음줄로 넘어간다.
- wrap-reverse : 순서가 반대로 wrap

### flex-flow

- flex-direction flex wrap 한 줄에 표현한다.

```css
/*flex-flow: flex-direction flex wrap;*/
flex-flow: column nowrap;
```

수직 방향으로 nowrap하겠다

### justify-content

주축에서 item을 어떻게 배치할것인지 (main axis)

- flex-start : 시작부터 배치(순서는 유지)
- flex-end : 끝부터 배치
- center : 중앙 배치
- space-around : item을 둘러싼 space를 만들어줌
- space-evenly : 똑같은 간격으로 둘러쌈
- space-between : 맨 왼쪽과 오른쪽은 딱 맞게 배치하고 중간에 space

### align-content

교차축의 정렬 방법(두 줄 이상)

- stretch : Container에 맞춰서 늘어남
- flex-start : 시작을 기준으로 정렬
- flex-end : 끝을 기준으로 정렬
- center : 중앙 배치
- space-between : 맨 위와 아래는 딱 맞게 배치하고 중간에 space
- space-around : item을 둘러싼 space를 만들어줌

### align-items

교차축의 정렬 방법(한 줄)

- stretch : Container에 맞춰서 늘어남
- flex-start : 시작을 기준으로 정렬
- flex-end : 끝을 기준으로 정렬
- center : 중앙 배치
- baseline : text가 균일하게 보여지도록

# Item

---

order/flex-grow/flex-shrink/flex-basis/align-self

### order

숫자로 위치를 지정할 수 있음(클수록 뒤에)

```css
order: 숫자;
```

### flex-grow

Container를 채울때 얼마나 커질지(비율로 지정)

### flex-shrink

Container가 작아졌을때 얼마나 작아질지(비율로 지정)

### flex-basis

item들의 크기 결정

- auto : flex-grow와 flex-shrink에 따라 변형
- 숫자 : 숫자에 따라서 비율이 지정

### align-self

아이템 별로 아이템을 정렬할 수 있음

##### [참고 자료 - https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Flexible_Box_Layout/Flexbox%EC%9D%98_%EA%B8%B0%EB%B3%B8_%EA%B0%9C%EB%85%90]
