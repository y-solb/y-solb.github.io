---
layout: post
title: "[Android] layout_width와 layout_height 속성"
date: 2020-10-26 08:17:36 +0530
categories: Android
---

layout_width와 layout_height는 필수 속성이다.

android:layout_width와 android:layout_height에서 android:는 생략이 가능하다.

layout_width - 가로 크기  
layout_height - 세로 크기

## 1. wrap_content

---

뷰에 들어 있는 내용의 크기에 자동으로 맞춘다.

## 2. match_parent

---

부모의 크기에 맞춰 꽉 채워진다.

## 3. 직접 크기 지정

---

숫자로 직접 크기를 지정한다. (px, dp와 같은 단위가 꼭 있어야한다.)

 <pre><code>android:layout_width="50dp"
android:layout_height="50dp"</code></pre>

##### [참고 자료 - do it! 안드로이드 앱 프로그래밍 7판]
