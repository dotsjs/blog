---
path: 'note02'
date: '2020-01-29 16:17:42'
title: '무작정 따라 하는 리액트 - Note(2)'
---

> **이 강좌는 지식을 얻는 것보다 무작정 따라 하며 코딩에 익숙해지는 것이 목적입니다.** 더 쉬운 설명을 위해 잘못된 설명이 있을 수 있습니다. 무작정 따라 하며 익숙해진 뒤 잘못된 부분들을 찾아보세요.

## 목차

1. [프로젝트 구조](#프로젝트-구조)
2. [컴포넌트 작성](#컴포넌트-작성)

## 프로젝트 구조

지금까지 codesandbox.io에서 작성할때는 모든 작업을 한 파일 안에서 작업을 했습니다. 하지만 실제 프로젝트를 할 때 가장 중요한 점은 `모듈화`입니다. 기능을 잘게 나눠서 모듈로 만드는 것이 좋은 프로그래밍의 핵심입니다.

우리가 작업할 것은 컴포넌트들이기 때문에 src폴더 내부에 components 폴더를 생성해주도록 하겠습니다.

```
note/
├── node_modules/
├── public/
├── src/
│   ├── components/
│   ├── App.css
│   ├── App.js
│   ├── App.test.js
│   ├── index.css
│   ├── index.js
│   ├── logo.svg
│   ├── serviceWorker.js
│   └── setupTests.js
├── .gitignore
├── package.json
├── README.md
└── yarn.lock
```

앞으로 필요한 폴더가 생길때마다 추가해주도록 하겠습니다.

## 컴포넌트 작성

리액트의 jsx가 사용되는 파일의 확장자는 .jsx로 변경해줍니다. 이 파일이 리액트 컴포넌트라는 것을 알아보기 쉽게 하기 위함입니다. `App.jsx`파일부터 천천히 컴포넌트를 만들어보도록 하겠습니다.

크게 가장 상위에서는 `Header`, `List`, `Note`로 나뉩니다. 하지만 css 작업을 위해 `List`와 `Note`를 `Main` 컴포넌트로 묶어서 작업해주도록 하겠습니다.

```jsx
function App() {
    return (
        <div className="App">
            <Header />
            <Main />
        </div>
    );
}
```

아직 컴포넌트를 만들어주지 않았기 때문에 에러가 날 것입니다. 이제 `components`폴더에 컴포넌트들을 작성해줍니다. 컴포넌트를 작성하기 위해서 해당 컴포넌트 이름으로 폴더를 만들고 내부에 `index.jsx`파일을 작성해주면 됩니다.

```
note/
├── src/
│   ├── components/
│   │   ├── Header/
│   │   │   └── index.jsx
│   │   └── Main/
│   │       └── index.jsx
...
```

이제 App컴포넌트에 해당 컴포넌트를 import해오면 됩니다.

```javascript
import Header from './components/Header';
import Main from './components/Main';
```

각 컴포넌트 역시 비슷한 방식으로 작업해주도록 하겠습니다. Header는 메뉴바, 제목, 추가버튼을 갖는 네비게이터 바 입니다.

```jsx
import React from 'react';

function Header() {
    return (
        <nav>
            <button>메뉴</button>
            <button>Note</button>
            <button>추가</button>
        </nav>
    );
}

export default Header;
```

새로 작성된 파일마다 컴포넌트를 만들어야한다면 react를 import 해와야 합니다. 뿐만아니라 컴포넌트를 외부에서 사용할 수 있도록 모듈화를 하기 위해 `export default 컴포넌트` 까지 작성해주어야 합니다. 만약 코드를 빠르게 작성할 수 있는 `snippet`이 `visual studio code`에 설치되었다면 `rfc`를 입력해서 빠르게 작성할 수 있습니다.

`Main`컴포넌트는 `List`와 `Note`를 포함하도록 합니다.

```jsx
function Main() {
    return (
        <div>
            <List />
            <Note />
        </div>
    );
}
```

`List`와 `Note`역시 같은 방식으로 컴포넌트를 생성해줍니다.

```jsx
import Item from './../Item';

export default function List() {
    return (
        <ul>
            <Item />
            <Item />
            <Item />
            <Item />
        </ul>
    );
}
```

`Item`컴포넌트 역시 만들어서 불러와주었습니다. 현재 디렉토리의 바깥에 있는 파일을 불러올 경우 `../`을 통해 상위 디렉토리에 접근할 수 있습니다. `./`는 현재 디렉토리를 의미합니다.

Item 컴포넌트

```jsx
function Item() {
    return <h3>제목</h3>;
}
```

Note 컴포넌트

```jsx
export default function Note() {
    return (
        <div>
            <h2>제목</h2>
            <p>내용</p>
            <button>수정</button>
        </div>
    );
}
```

추가 버튼이나 수정 버튼을 누를 시 편집할 수 있는 컴포넌트가 Note 대신 나와야하기 때문에 편집 컴포넌트도 작성해 두도록 하겠습니다.

Edit 컴포넌트

```jsx
function Edit() {
    return (
        <form>
            <input type="text" placeholder="제목" />
            <textarea />
            <input type="submit" value="확인" />
        </form>
    );
}
```
