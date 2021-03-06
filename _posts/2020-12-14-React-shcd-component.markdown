---
layout: post
title: "리액트 Component"
date: 2020-12-14 19:48:36 +0530
categories: React
---

# Component

- 가독성
- 재사용성
- 유지보수

<br>

## 일반 html파일

---

```html
<html>
  <body>
    <header>
      <h1>WEB</h1>
      World Wide Web!
    </header>

    <nav>
      <ul>
        <li><a href="1.html">HTML</a></li>
        <li><a href="2.html">CSS</a></li>
        <li><a href="3.html">JavaScript</a></li>
      </ul>
    </nav>

    <article>
      <h2>HTML</h2>
      HTML is HyperText Markup Language.
    </article>
  </body>
</html>
```

<br>

## React로 변경

### App.js

---

```javascript
import "./App.css";
import Subject from "./components/Subject"; //components폴더 아래 Subject파일에서 Subject를 import
import TOC from "./components/TOC";
import Content from "./components/Content";
import { Component } from "react";

class App extends Component {
  render() {
    return (
      <div className="App">
        <Subject title="WEB" sub="world wide web!"></Subject>
        <Subject title="React" sub="for UI!"></Subject>
        <TOC></TOC>
        <Content title="HTML" desc="HTML is HyperText Markup Language. UI!"></Content>
      </div>
    );
  }
}

export default App;
```

pros(properties)  
어떤 값을 component에게 전달할때 사용한다.

### Subject.js

---

```javascript
import React, { Component } from "react";

class Subject extends Component {
  //Subject라는 Component를 만들겠다.
  render() {
    //함수(function 생략)
    return (
      <header>
        <h1>{this.props.title}</h1>
        {this.props.sub}
      </header> //Component는 하나의 최상위 태그만(header)
    );
  }
}

export default Subject; //Subject를 내보내기
```

### TOC.js

---

```javascript
import React, { Component } from "react";

class TOC extends Component {
  render() {
    return (
      <nav>
        <ul>
          <li>
            <a href="1.html">HTML</a>
          </li>
          <li>
            <a href="2.html">CSS</a>
          </li>
          <li>
            <a href="3.html">JavaScript</a>
          </li>
        </ul>
      </nav>
    );
  }
}

export default TOC;
```

### Content.js

---

```javascript
import React, { Component } from "react";

class Content extends Component {
  render() {
    return (
      <article>
        <h2>{this.props.title}</h2>
        {this.props.desc}
      </article>
    );
  }
}

export default Content; //외부에서 사용할 수 있도록
```

component는 언제나 똑같이 띄워줌
pros는 다른 값을 띄워줌 사용자 정의 태그?

##### [생활코딩-react강의]
