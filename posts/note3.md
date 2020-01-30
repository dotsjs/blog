---
path: 'note03'
date: '2020-01-30 00:17:44'
title: '무작정 따라 하는 리액트 - Note(3)'
---

> **이 강좌는 지식을 얻는 것보다 무작정 따라 하며 코딩에 익숙해지는 것이 목적입니다.** 더 쉬운 설명을 위해 잘못된 설명이 있을 수 있습니다. 무작정 따라 하며 익숙해진 뒤 잘못된 부분들을 찾아보세요.

## 목차

1. [데이터 구조](#데이터-구조)
2. [state 선언](#state-선언)
3. [함수 선언](#함수-선언)

## 데이터 구조

Note앱의 핵심인 Note의 구조를 정하고 임시데이터를 넣어보도록 하겠습니다. 지금 보이는 것만 생각해 보았을 때 Note는 제목과 내용으로 이루어져있습니다. 제목과 내용만으로 구성을 해도 되지만 같은 구조들이 여러 묶음으로 구성되어 나열해야할 경우엔 각각을 구분할 수 있는 정보가 필요합니다. 제목과 내용만으로는 구분하기가 쉽지 않습니다. 그렇기 때문에 구분을 위해 고유한 식별자를 추가하도록 하겠습니다.

```javascript
note = {
    id: '01',
    title: '제목1',
    content: '내용1',
};
```

이제 임시로 데이터를 구성해보도록 하겠습니다.

## state 선언

데이터를 선언하기 위해선 어느 컴포넌트에서 선언해야하는 지 충분히 고민을 해봐야합니다. note들은 List에서 목록을 보여주기위해 사용되고 Note에서 내용을 보여주기 위해 사용됩니다. 각각의 컴포넌트들의 공통된 부모 컴포넌트에서 state를 생성해주면 됩니다. List와 Note의 공통 부모 컴포넌트는 Main 컴포넌트 입니다.

```javascript
import React, { useState } from 'react';

function Main() {
    const [notes, setNotes] = useState([
        {
            id: '01',
            title: '제목1',
            content: '내용1',
        },
        {
            id: '02',
            title: '제목2',
            content: '내용2',
        },
        {
            id: '03',
            title: '제목3',
            content: '내용3',
        },
        {
            id: '04',
            title: '제목4',
            content: '내용4',
        },
    ]);
}
```

임시로 4개의 데이터를 만들어서 넣어주도록 하겠습니다. 이제 notes가 필요한 컴포넌트에 props로 전달해주면 됩니다.

## 함수 선언

이제 note들을 조작할 수 있는 함수를 만들어주도록 하겠습니다. 기본적으로 추가, 수정, 삭제 만 만들도록 하겠습니다.

### 추가

사용자에게 입력을 받는 제목, 내용은 함수의 인자로 받아서 추가하는 함수를 작성해야합니다.

```javascript
function addNote(title, content) {}
```

사용자에게 입력받지 않는 id값을 생성해주어야 하는데 다른 것과 겹치지 않는 고유한 값을 만들어야 합니다. 여러 방법들이 있지만 가장 쉬운 방법인 생성 함수가 호출된 시각을 이용해 고유한 값을 만들도록 하겠습니다. 현재 시각은 여러 사용자가 동시에 사용하지 않는 이상 나름대로 안전하게 고유한 값을 만들 수 있습니다. 현재 시각은 자바스크립트의 Date 객체를 통해 얻을 수 있습니다.

```javascript
function addNote(title, content) {
    const id = Date.now();
}
```

이제 3개의 값을 이용해 notes에 추가해주면됩니다.

```javascript
function addNote(title, content) {
    const id = Date.now();
    setNotes(prev => [...prev, { id, title, content }]);
}
```

객체를 생성하기 위해서는 `{ 키 : 값 }` 으로 생성할 수 있습니다. `키`는 문자열이고 `값`은 어떤 값이나 변수가 될 수 있습니다. 이때 `값`에 변수가 사용되고 `키`와 변수명이 같다면 `{ 키 }`로 줄여서 표현할 수 있습니다.

### 수정, 삭제

수정이나 삭제를 위해서는 어떠한 노트를 삭제할 것인지를 알아야합니다. 배열에 담겨져 있기 때문에 결과적으로 해당 index를 알아내야합니다. 하지만 그 index는 해당 note에게 고유한 값이 아닙니다.

예를들어 각 index를 고유값으로 갖는 note 3가지가 있다고 가정해봅시다.
0|1|2
-:|-:|-:
note0|note1|note2

이때 1번 index에 위치한 note가 삭제된다면
0|1
-:|-:
note0|note2
의 모습이 되어 1번 index는 2번 note를 갖게 되어 매칭이 안됩니다.

이처럼 index는 고유의 값을 가질 수 없기 때문에 index가 아닌 값으로 해당 index를 찾아야합니다. 지금 각 note는 고유한 id값을 갖고 있기 때문에 id를 이용해 찾도록 하겠습니다.

```javascript
function indexOfNote(id) {
    return notes.findIndex(note => note.id === id);
}
```

자바스크립트의 findIndex함수를 이용해 인자로 받은 id값이 같은 note의 index를 찾았습니다. 이제 이 index를 이용해 수정과 삭제를 만들어주시면 됩니다.

```javascript
function modifyNote(id, title, content) {
    const index = indexOfNote(id);
    setNotes(prev => [...prev.slice(0, index), { id, title, content }, ...prev.slice(index + 1)]);
}
```

```javascript
function deleteNote(id) {
    const index = indexOfNote(id);
    setNotes(prev => [...prev.slice(0, index), ...prev.slice(index + 1)]);
}
```
