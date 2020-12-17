---
layout: post
title: "[React] Props와 State"
date: 2020-12-16 14:26:11 +0530
categories: React
---

# Props(Properties)

-   속성을 나타내는 데이터
-   component를 외부에서 조작할 때 사용(사용자에게 공개)

# State

-   내부적으로 상태를 관리할 때 사용
    <br>

## 일반 html파일

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

-   state 추가

```javascript
import "./App.css";
import Subject from "./components/Subject"; //components폴더 아래 Subject파일에서 Subject를 import
import TOC from "./components/TOC";
import Content from "./components/Content";
import { Component } from "react";

class App extends Component {
    constructor(props) {
        // render()보다 먼저 실행이 됨 state의 값을 초기화
        super(props);
        this.state = {
            subject: { title: "WEB", sub: "World Wide Web!" },
            contents: [
                { id: 1, title: "HTML", desc: "HTML is for information" },
                { id: 2, title: "CSS", desc: "CSS is for design" },
                { id: 3, title: "JavaScript", desc: "JavaScript is for interactive" },
            ],
        };
    }
    render() {
        return (
            <div className="App">
                <Subject title={this.state.subject.title} sub={this.state.subject.sub}></Subject>
                <TOC data={this.state.contents}></TOC>
                <Content title="HTML" desc="HTML is HyperText Markup Language. UI!"></Content>
            </div>
        );
    }
}

export default App;
```

### Subject.js

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

-   state 추가

```javascript
import React, { Component } from "react";

class TOC extends Component {
    render() {
        var lists = [];
        var data = this.props.data;
        var i = 0;
        while (i < data.length) {
            lists.push(
                <li key={data[i].id}>
                    <a href={"/content/" + data[i].id}>{data[i].title}</a>
                </li>
            );
            i = i + 1;
        }
        return (
            <nav>
                <ul>{lists}</ul>
            </nav>
        );
    }
}

export default TOC;
```

### Content.js

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

##### [생활코딩-react강의]
