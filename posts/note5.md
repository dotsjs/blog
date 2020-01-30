---
path: 'note05'
date: '2020-01-30 23:42:28'
title: '무작정 따라 하는 리액트 - Note(5)'
---

> **이 강좌는 지식을 얻는 것보다 무작정 따라 하며 코딩에 익숙해지는 것이 목적입니다.** 더 쉬운 설명을 위해 잘못된 설명이 있을 수 있습니다. 무작정 따라 하며 익숙해진 뒤 잘못된 부분들을 찾아보세요.

## 목차

1. [스타일링](#스타일링)
2. [Header](#header)
3. [Main](#main)
4. [List](#list)
5. [Item](#item)
6. [Note](#note)
7. [Edit](#edit)

## 스타일링

스타일링하는 여러가지 방법이 있지만 여기서는 단순히 src의 App.css에 작업하도록 하겠습니다. App.css의 내용은 전부 삭제한뒤 전체 높이를 차지하기 위해 아래 코드를 작성해줍니다.

```css
html,
body,
#root,
.App {
    height: 100vh;
    overflow: hidden;
}
```

스타일이 필요한 엘리먼트에는 className 속성을 적용해서 class와 같이 불러올 수 있습니다. css는 자세한 설명없이 코드만 보여드리도록 하겠습니다.

## Header

Header에는 왼쪽 bar아이콘과 오른쪽 plus아이콘이 들어가는데 이는 <fontawesome.com> 의 아이콘을 이용했습니다.
이를 위해 터미널을 이용해 해당 라이브러리를 받아와야합니다.

```bash
npm i --save @fortawesome/fontawesome-svg-core  @fortawesome/free-solid-svg-icons @fortawesome/react-fontawesome
```

이제 Header 컴포넌트를 icon을 이용해 변경해줍니다.

```jsx
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faBars, faPlus } from '@fortawesome/free-solid-svg-icons';

function Header({ setEditing }) {
    return (
        <nav>
            <FontAwesomeIcon icon={faBars} />
            <button>Note</button>
            <FontAwesomeIcon onClick={() => setEditing(true)} icon={faPlus} />
        </nav>
    );
}
```

```css
nav {
    background: #fdcb6e;
    padding: 1rem;
    display: flex;
    align-items: center;
    font-size: 1.6rem;
    color: white;
}

nav > button {
    border: none;
    background: none;
    margin: auto;
    font-size: 2rem;
    cursor: pointer;
    outline: none;
}

nav > svg {
    cursor: pointer;
}
```

## Main

```css
main {
    height: calc(100% - 3.6rem);
    display: flex;
}
```

## List

```css
main > ul {
    margin: 0;
    padding: 0;
    background: #e17055;
    min-width: 200px;
}
```

## Item

```css
main > ul > li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 0.8rem;
    border-bottom: 1px solid #fdcb6e;
}
main > ul > li > h3 {
    margin: 0;
}
```

## Note

```css
main > article {
    flex: 1;
    padding: 1em;
    display: flex;
    flex-direction: column;
}

main > article > h2 {
    margin: 0;
    padding-bottom: 0.5em;
    border-bottom: 1px solid black;
}
main > article > p {
    flex: 1;
    overflow-y: scroll;
}

main > article > button {
    border: 2px solid #fdcb6e;
    border-radius: 2em;
    background: white;
    padding: 0.4em 0.6em;
    font-size: 1.2em;
    margin: 1em;
}
```

## Edit

```css
main > form {
    flex: 1;
    padding: 1em;
    display: flex;
    flex-direction: column;
}

main > form > input[type='text'] {
    margin: 0;
    padding: 0;
    font-size: 1.5rem;
    font-weight: bold;
    padding-bottom: 0.7em;
    border: none;
    border-bottom: 1px solid black;
}
main > form > textarea {
    flex: 1;
    overflow-y: scroll;
    border: none;
    font-size: 1rem;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen', 'Ubuntu', 'Cantarell', 'Fira Sans',
        'Droid Sans', 'Helvetica Neue', sans-serif;
    margin: 1rem 0;
    padding: 0;
    resize: none;
}

main > form > input[type='submit'] {
    border: 2px solid #fdcb6e;
    border-radius: 2em;
    background: white;
    padding: 0.4em 0.6em;
    font-size: 1.2em;
    margin: 1em;
}
```
