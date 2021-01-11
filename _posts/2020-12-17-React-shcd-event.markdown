---
layout: post
title: "[React] Event"
date: 2020-12-17 15:30:08 +0530
categories: React
---

# Event

-   소문자 대신 캐멀 케이스(camelCase)를 사용
-   JSX를 사용하여 문자열이 아닌 함수로 이벤트 핸들러를 전달

상위 컴포가 하위로 값을 전달할 때는 props를 통해 전달  
하위 컴포가 상위 컴포의 값을 전달할 때는 event를 통해 상위 state의 값의 호출을 통해 바뀜

redux는 하나의 데이터 저장소가 있어 관련 데이터가 바뀌면 다같이 바뀜

<br>

## React로 변경

### App.js

---

-   event 추가

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
            mode: "read",
            selected_content_id: 2,
            subject: { title: "WEB", sub: "World Wide Web!" },
            welcome: { title: "Welcome", desc: "Hello, React!!" },
            contents: [
                { id: 1, title: "HTML", desc: "HTML is for information" },
                { id: 2, title: "CSS", desc: "CSS is for design" },
                { id: 3, title: "JavaScript", desc: "JavaScript is for interactive" },
            ],
        };
    }
    render() {
        var _title,
            _desc = null;
        if (this.state.mode === "welcome") {
            _title = this.state.welcome.title;
            _desc = this.state.welcome.desc;
        } else if (this.state.mode === "read") {
            var i = 0;
            while (i < this.state.contents.length) {
                var data = this.state.contents[i];
                if (data.id === this.state.selected_content_id) {
                    _title = data.title;
                    _desc = data.desc;
                    break;
                }
                i = i + 1;
            }
        }
        return (
            <div className="App">
                <Subject
                    title={this.state.subject.title}
                    sub={this.state.subject.sub}
                    onChangePage={function () {
                        this.setState({ mode: "welcome" });
                    }.bind(this)} // event
                ></Subject>
                {/* <header>
            <h1><a href="/" onClick={function(e){ // event
              console.log(e);
              e.preventDefault();
              // this.state.mode = 'welcome'; 리액트가 값이 바뀐지 모름(화면이 바뀌지 않음)
              this.setState({
                mode : 'welcome'
              }); // 동적으로 생성된 값을 바꿀 때는 this.setState(변경하고 싶은 값);
            }.bind(this)}>{this.state.subject.title}</a></h1> 
            {this.state.subject.sub} 
          </header>*/}
                <TOC
                    onChangePage={function (id) {
                        this.setState({
                            mode: "read",
                            selected_content_id: Number(id),
                        });
                    }.bind(this)}
                    data={this.state.contents}
                ></TOC>
                <Content title={_title} desc={_desc}></Content>
            </div>
        );
    }
}

export default App;
```

setState : state값을 직접 변경하면 안되고 setState를 사용해야함 ---> 리액트가 값이 바뀐지 모름(화면이 바뀌지 않음)  
 동적으로 생성된 값을 바꿀 때는 this.setState(변경하고 싶은 값); ---> 리액트가 내부적으로 많은 일을 할 수 있도록

### Subject.js

---

-   event 추가

```javascript
import React, { Component } from "react";

class Subject extends Component {
    //Subject라는 Component를 만들겠다.
    render() {
        //함수(function 생략)
        return (
            <header>
                <h1>
                    <a
                        href="/"
                        onClick={function (e) {
                            // 첫번째 인자로 이벤트 객체가 전달
                            e.preventDefault(); // 페이지가 바뀌는 것을 막음
                            this.props.onChangePage(); // 함수 호출
                        }.bind(this)}
                    >
                        {this.props.title}
                    </a>
                </h1>
                {this.props.sub}
            </header> //Component는 하나의 최상위 태그만(header)
        );
    }
}

export default Subject; //Subject를 내보내기
```

### TOC.js

---

-   event 추가

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
                    <a
                        href={"/content/" + data[i].id}
                        data-id={data[i].id}
                        onClick={function (e) {
                            e.preventDefault();
                            this.props.onChangePage(e.target.dataset.id);
                        }.bind(this)}
                    >
                        {data[i].title}
                    </a>
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

-   간략하게

---

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
                    <a
                        href={"/content/" + data[i].id}
                        onClick={function (id, e) {
                            e.preventDefault();
                            this.props.onChangePage(id);
                        }.bind(this, data[i].id)}
                    >
                        {data[i].title}
                    </a>
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

.bind는 함수 내부에서 this의 값을 넣어줌  
.bind(this로 넣고 싶은 값의 변수)

##### [생활코딩-react강의]

##### [https://ko.reactjs.org/docs/handling-events.html]
