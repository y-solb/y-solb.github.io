---
layout: post
title: "[React] Update와 Delete"
date: 2020-12-20 23:45:19 +0530
categories: React
---

### App.js

---

```javascript
import "./App.css";
import Subject from "./components/Subject"; //components폴더 아래 Subject파일에서 Subject를 import
import TOC from "./components/TOC";
import ReadContent from "./components/ReadContent";
import Control from "./components/Control";
import CreateContent from "./components/CreateContent";
import UpdateContent from "./components/UpdateContent";
import { Component } from "react";

class App extends Component {
    constructor(props) {
        // render()보다 먼저 실행이 됨 state의 값을 초기화
        super(props);
        this.max_content_id = 3; // 마지막 content의 아이디 값
        this.state = {
            mode: "welcome",
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
    getReadConent() {
        var i = 0;
        while (i < this.state.contents.length) {
            var data = this.state.contents[i];
            if (data.id === this.state.selected_content_id) {
                return data;
            }
            i = i + 1;
        }
    }
    getContent() {
        var _title,
            _desc,
            _article = null;
        if (this.state.mode === "welcome") {
            _title = this.state.welcome.title;
            _desc = this.state.welcome.desc;
            _article = <ReadContent title={_title} desc={_desc}></ReadContent>;
        } else if (this.state.mode === "read") {
            var _content = this.getReadConent();
            _article = <ReadContent title={_content.title} desc={_content.desc}></ReadContent>;
        } else if (this.state.mode === "create") {
            _article = (
                <CreateContent
                    onSubmit={function (_title, _desc) {
                        this.max_content_id = this.max_content_id + 1;
                        var _contents = Array.from(this.state.contents);
                        _contents.push({ id: this.max_content_id, title: _title, desc: _desc });
                        this.setState({
                            contents: _contents,
                            mode: "read",
                            selected_content_id: this.max_content_id,
                        });
                    }.bind(this)}
                ></CreateContent>
            );
        } else if (this.state.mode === "update") {
            _content = this.getReadConent();
            _article = (
                <UpdateContent
                    data={_content}
                    onSubmit={function (_id, _title, _desc) {
                        var _contents = Array.from(this.state.contents);
                        var i = 0;
                        while (i < _contents.length) {
                            if (_contents[i].id === _id) {
                                _contents[i] = { id: _id, title: _title, desc: _desc };
                                break;
                            }
                            i = i + 1;
                        }
                        this.setState({
                            contents: _contents,
                            mode: "read",
                        });
                    }.bind(this)}
                ></UpdateContent>
            );
        }
        return _article;
    }
    render() {
        console.log("App render");
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
                        if (_mode === "delete") {
                            if (window.confirm("really??")) {
                                var _contents = Array.from(this.state.contents);
                                var i = 0;
                                while (i < _contents.length) {
                                    if (_contents[i].id === this.state.selected_content_id) {
                                        _contents.splice(i, 1);
                                        break;
                                    }
                                    i = i + 1;
                                }
                                this.setState({
                                    mode: "welcome",
                                    contents: _contents,
                                });
                                alert("delete successfully");
                            }
                        } else {
                            this.setState({
                                mode: _mode,
                            });
                        }
                    }.bind(this)}
                ></Control>
                {this.getContent()}
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

### UpdateContent.js

---

```javascript
import React, { Component } from "react";

class UpdateContent extends Component {
    constructor(props) {
        super(props);
        this.state = {
            id: this.props.data.id,
            title: this.props.data.title,
            desc: this.props.data.desc,
        };
        this.inputFormHandler = this.inputFormHandler.bind(this);
    }
    inputFormHandler(e) {
        this.setState({ [e.target.name]: e.target.value });
    }
    render() {
        console.log("UpdateContent render");
        console.log(this.props.data);
        return (
            <article>
                <h2>Update</h2>
                <form
                    action="/create_process"
                    method="post"
                    onSubmit={function (e) {
                        e.preventDefault();
                        this.props.onSubmit(this.state.id, this.state.title, this.state.desc);
                    }.bind(this)}
                >
                    <input type="hidden" name="id" value={this.state.id}></input>
                    <p>
                        <input
                            type="text"
                            name="title"
                            placeholder="title"
                            value={this.state.title}
                            onChange={this.inputFormHandler}
                        ></input>
                    </p>
                    <p>
                        <textarea
                            onChange={this.inputFormHandler}
                            name="desc"
                            placeholder="description"
                            value={this.state.desc}
                        ></textarea>
                    </p>
                    <p>
                        <input type="submit"></input>
                    </p>
                </form>
            </article>
        );
    }
}

export default UpdateContent;
```

##### [생활코딩-react강의]
