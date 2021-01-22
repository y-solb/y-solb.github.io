---
layout: post
title: "Create"
date: 2020-12-19 10:17:48 +0530
categories: React
---

### App.js

---

CreateContent생성

push - 원본을 바꿈  
concat -원본이 바뀌지 않음(원본의 복제본에 값을 넣어준다)

```javascript
import "./App.css";
import Subject from "./components/Subject"; //components폴더 아래 Subject파일에서 Subject를 import
import TOC from "./components/TOC";
import ReadContent from "./components/ReadContent";
import Control from "./components/Control";
import CreateContent from "./components/CreateContent";
import { Component } from "react";

class App extends Component {
  constructor(props) {
    // render()보다 먼저 실행이 됨 state의 값을 초기화
    super(props);
    this.max_content_id = 3; // 마지막 content의 아이디 값
    this.state = {
      mode: "create",
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
      _desc,
      _article = null;
    if (this.state.mode === "welcome") {
      _title = this.state.welcome.title;
      _desc = this.state.welcome.desc;
      _article = <ReadContent title={_title} desc={_desc}></ReadContent>;
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
      _article = <ReadContent title={_title} desc={_desc}></ReadContent>;
    } else if (this.state.mode === "create") {
      _article = (
        <CreateContent
          onSubmit={function (_title, _desc) {
            // add content to this.state.contents
            this.max_content_id = this.max_content_id + 1;
            // this.state.contents.push({id:this.max_content_id, title:_title, desc:_desc}
            // );
            var _contents = this.state.contents.concat({
              id: this.max_content_id,
              title: _title,
              desc: _desc,
            });
            this.setState({
              contents: _contents,
            });
          }.bind(this)}
        ></CreateContent>
      );
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
        <TOC
          onChangePage={function (id) {
            this.setState({
              mode: "read",
              selected_content_id: Number(id),
            });
          }.bind(this)}
          data={this.state.contents}
        ></TOC>
        <Control
          onChangeMode={function (_mode) {
            this.setState({
              mode: _mode,
            });
          }.bind(this)}
        ></Control>
        {_article}
      </div>
    );
  }
}

export default App;
```

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

shouldComponentupdate - render이전에 실행된다  
true면 render호출되고  
false면 render가 호출되지 않는다

새롭게 바뀐값, 이전값에 접근 가능  
원본을 바꾸지 않는다 = 불변성 (immutable) ---> 성능을 위해 필요

```javascript
import React, { Component } from "react";

class TOC extends Component {
  shouldComponentUpdate(newProps, newState) {
    console.log("===> TOC render shouldComponentUpdate", newProps.data, this.props.data);
    if (this.props.data === newProps.data) {
      return false;
    }
    return true;
  }
  render() {
    console.log("===>TOC render");
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

### Control.js

---

create / update / delete

```javascript
import React, { Component } from "react";

class Control extends Component {
  render() {
    return (
      <ul>
        <li>
          <a
            href="/create"
            onClick={function (e) {
              e.preventDefault();
              this.props.onChangeMode("create");
            }.bind(this)}
          >
            create
          </a>
        </li>
        <li>
          <a
            href="/update"
            onClick={function (e) {
              e.preventDefault();
              this.props.onChangeMode("update");
            }.bind(this)}
          >
            update
          </a>
        </li>
        <li>
          <input
            onClick={function (e) {
              e.preventDefault();
              this.props.onChangeMode("delete");
            }.bind(this)}
            type="button"
            value="delete"
          ></input>
        </li>
      </ul>
    );
  }
}

export default Control;
```

### CreateContent.js

---

```javascript
import React, { Component } from "react";

class CreateContent extends Component {
  render() {
    return (
      <article>
        <h2>Create</h2>
        <form
          action="/create_process"
          method="post"
          onSubmit={function (e) {
            e.preventDefault();
            this.props.onSubmit(e.target.title.value, e.target.desc.value);
          }.bind(this)}
        >
          <p>
            <input type="text" name="title" placeholder="title"></input>
          </p>
          <p>
            <textarea name="desc" placeholder="description"></textarea>
          </p>
          <p>
            <input type="submit"></input>
          </p>
        </form>
      </article>
    );
  }
}

export default CreateContent;
```

### ReadContent.js

---

```javascript
import React, { Component } from "react";

class ReadContent extends Component {
  render() {
    return (
      <article>
        <h2>{this.props.title}</h2>
        {this.props.desc}
      </article>
    );
  }
}

export default ReadContent;
```

##### [생활코딩-react강의]
