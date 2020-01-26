---
path: 'react02'
date: '2020-01-26 16:54:20'
title: '무작정 따라 하는 리액트 - 변수'
---

> **이 강좌는 지식을 얻는 것보다 무작정 따라 하며 코딩에 익숙해지는 것이 목적입니다.** 더 쉬운 설명을 위해 잘못된 설명이 있을 수 있습니다. 무작정 따라 하며 익숙해진 뒤 잘못된 부분들을 찾아보세요.

## 목차

1. [변수 선언하기](#변수-선언하기)
2. [State](#State)

## 변수 선언하기

일반적으로 변수를 사용하기 위해서는 `let`이라는 키워드를 사용할 수 있습니다. 해당 변수가 primitive type(number, string, boolean)이 아니라면 `const` 키워드를 사용해줍니다.

변수와 이벤트에 대해 실습하기 위해 버튼을 누르면 화면에 표시된 숫자가 증가하는 웹을 만들어 보도록 하겠습니다.

```jsx
function App() {
    return (
        <div>
            <h1>숫자가 표시될 곳</h1>
            <button>증가 버튼</button>
        </div>
    );
}
```

위와 같이 엘리먼트들을 작성해줍니다. 이제 숫자를 표시하기 위해서 변수를 선언하고 화면에 표시하도록 하겠습니다.

```jsx
function App() {
    let count = 0;
    return (
        <div>
            <h1>{count}</h1>
            <button>증가 버튼</button>
        </div>
    );
}
```

JSX에서 자바스크립트를 사용하기 위해서 `{}`를 사용해 주었습니다.

이제 버튼이 `click`되었을 때 값이 증가하는 이벤트 처리를 해주도록 하겠습니다.

해당 엘리먼트 속성에 이벤트 리스너를 넣어주고 이벤트 리스너에는 이벤트 핸들러 함수를 작성해줍니다.

```jsx
function App() {
    let count = 0;
    const clickHandler = () => {
        count++;
    };
    return (
        <div>
            <h1>{count}</h1>
            <button onClick={clickHandler}>증가 버튼</button>
        </div>
    );
}
```

눌릴 때 count값이 1 증가하는 함수를 만들었습니다. 이제 버튼이 눌려지면 clickHandler가 수행되어 count의 값이 1증가하게 될 것입니다. 하지만 아무리 버튼을 눌러도 화면의 count는 증가하지 않습니다. 실제로도 그런 것인지 console을 통해 확인해보겠습니다.

```jsx
function App() {
    let count = 0;
    const clickHandler = () => {
        count++;
        console.log(count);
    };
    return (
        <div>
            <h1>{count}</h1>
            <button onClick={clickHandler}>증가 버튼</button>
        </div>
    );
}
```

실행됐을 때 화면에서 왼쪽아래에 console을 볼 수 있는 창이 있습니다. 실제로는 버튼이 눌렸을 때 count가 증가하는 것을 확인할 수 있습니다.

## State

변경된 값이 화면에서도 변경되기 위해서는 화면을 다시 그려주는 작업이 필요합니다. 이를 렌더링이라고 표현하는데 다시 렌더링을 해주어야 하는 것이죠. 리액트에서 화면을 다시 렌더링하기 위해서는 `state`라는 것이 필요합니다. 리액트는 화면이 전체 렌더링 되는 것을 막고 변경된 것만 렌더링하는 특징이 있습니다. 변경된 것을 알아차리기 위해 변하는 값을 단순히 선언하는 것이 아니라 리액트가 알기 쉽도록 `state`라는 것으로 선언해주어야 합니다.

```javascript
import React, { useState } from 'react';
```

`state`를 사용하기 위해 위와 같이 불러옵니다. 이제 화면에서 변경되는 값을 선언할 때는 아래와 같이 선언해줍니다.

```javascript
function App(){
    const [state, setState] = useState(0);
    ...
}
```

`const [변수, 해당 변수를 변경할 수 있는 함수] = useState(초기값);`  
변수의 값을 변경하기 위해서는 위의 useState로부터 반환받은 해당 변수를 변경할 수 있는 함수를 통해서 변경해주어야 합니다. 다시 위의 count 를 `state`로 변경해보겠습니다.

```jsx
function App() {
    const [count, setCount] = useState(0);
    const clickHandler = () => {
        setCount(count + 1);
    };
    return (
        <div>
            <h1>{count}</h1>
            <button onClick={clickHandler}>증가 버튼</button>
        </div>
    );
}
```

변수를 변경할 수 있는 함수에는 그 변수를 어떤 값으로 변경시킬 건지를 파라미터로 전달해 주어야 합니다. 이제 화면에도 잘 표시되는 것을 확인할 수 있습니다.

변수를 변경할 수 있는 함수에는 값을 전달해서 해당 변수를 변경시킬수도 있지만 함수를 전달할 수 도 있습니다. 그 함수는 현재 변수의 값을 매개변수로 받아 새로운 값을 리턴하는 형태를 띕니다. 즉 현재의 값을 이용해서 새로운 값으로 변경되어야 하는 경우에는 위처럼 `count + 1`을 넣어주는 것이 아니라 함수를 작성해주는 것이 좋습니다.
`setCount(prev => prev+1)`현재의 값을 가져와서 +1한 값을 return한 것입니다.
