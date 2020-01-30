---
path: 'note04'
date: '2020-01-30 22:22:22'
title: '무작정 따라 하는 리액트 - Note(4)'
---

> **이 강좌는 지식을 얻는 것보다 무작정 따라 하며 코딩에 익숙해지는 것이 목적입니다.** 더 쉬운 설명을 위해 잘못된 설명이 있을 수 있습니다. 무작정 따라 하며 익숙해진 뒤 잘못된 부분들을 찾아보세요.

## 목차

1. [props 전달](#props-전달)
2. [이벤트 연결](#이벤트-연결)

## props 전달

이제 Main 컴포넌트에 작성된 state를 필요한 곳에 props로 전달해주어야합니다. List에서는 notes가 전부 필요하고 Note는 선택된 note가 필요하기 때문에 무엇이 선택되었는 지 알아낸 뒤 전달하도록 하겠습니다. 무엇이 선택되었는지에 대한 변수 selected는 변경될 수 있기 때문에 state로 선언해 주도록 하겠습니다.

```jsx
function Main() {
    const [selected, setSelected] = useState(0);
    ...
    return (
        <div>
            <List notes={notes} />
            <Note note={notes[selected]} />
        </div>
    );
}
```

List에서 어떤 Note를 고를 지 선택 할 수 있고 삭제할 수 있기 때문에 note를 선택하는 함수를 만들고 삭제 함수와 함께 전달해주도록 하겠습니다.

```jsx
function Main() {
    const [selected, setSelected] = useState(0);
    ...
    const selectNote = id => {
        const index = indexOfNote(id);
        setSelected(index);
    };
    return (
        <div>
            <List notes={notes} selectNote={selectNote} deleteNote={deleteNote} />
            <Note note={notes[selected]} />
        </div>
    );
}
```

Note 컴포넌트는 List에서 노트가 선택되었을 때 보여지는 화면입니다. 만약 수정버튼이나 추가 버튼이 눌릴 경우 Edit 컴포넌트가 보여지게 됩니다. 하지만 추가 버튼은 Header에 있기 때문에 Main과 Header의 공통 부모 컴포넌트인 App 컴포넌트에서 작성해 주도록 하겠습니다.

```jsx
function App() {
    const [editing, setEditing] = useState(false);
    return (
        <div className="App">
            <Header setEditing={setEditing} />
            <Main editing={editing} setEditing={setEditing} />
        </div>
    );
}
```

이제 Main컴포넌트는 editing과 setEditing을 props로 전달받아서 처리를 해주면 됩니다. Edit 컴포넌트에는 추가, 수정 함수를 전달해주도록 하겠습니다.

```jsx
function Main({ editing, setEditing }) {
    return (
        <div>
            <List notes={notes} selectNote={selectNote} deleteNote={deleteNote} />
            {editing ? (
                <Edit addNote={addNote} modifyNote={modifyNote} />
            ) : (
                <Note note={notes[selected]} setEditing={setEditing} />
            )}
        </div>
    );
}
```

지금까지 전달한 props들을 받는 처리를 해주도록 하겠습니다. visual studio code에서는 ctrl을 누른 상태로 해당 컴포넌트를 선택하면 해당 파일로 이동할 수 있습니다.

#### Header

```javascript
function Header({ setEditing }) {}
```

#### List

```javascript
function List({ notes, selectNote, deleteNote }) {}
```

#### Edit

```javascript
function Edit({ addNote, modifyNote }) {}
```

#### Note

```javascript
function Note({ note, setEditing }) {}
```

## 이벤트 연결

전달받은 값과 함수들을 사용자의 행위에 따라 작동하게 하기 위해 이벤트를 연결해주도록 하겠습니다.

### Header

추가 버튼 선택 시 editing이 `true`가 되면 됩니다.

```jsx
<button onClick={() => setEditing(true)}>추가</button>
```

### Edit

form을 작성하기 위해 입력받을 수 있는 text들을 state로 선언해 주어야 합니다. 이 state들은 사용자가 input창에 입력시 변경되도록 해줍니다. 뿐만아니라 form의 전송이 발생했을 때 처리할 함수도 작성해주도록 하겠습니다.

```jsx
function Edit({ addNote, modifyNote }) {
    const [title, setTitle] = useState('');
    const [content, setContent] = useState('');
    const submitHandler = e => {
        e.preventDefault();
    };
    return (
        <form onSubmit={submitHandler}>
            <input type="text" placeholder="제목" value={title} onChange={e => setTitle(e.target.value)} />
            <textarea placeholder="내용" value={content} onChange={e => setContent(e.target.value)} />
            <input type="submit" value="확인" />
        </form>
    );
}
```

기본적인 형태의 form을 만들어 준 뒤 submitHandler에 함수를 연결해줍니다. 하지만 추가인지 수정인지 알 수 없기 때문에 그것을 알 수 있는 무언가가 필요합니다. 그래서 `modifying`이라는 변수를 받고 수정인 경우 note의 id값이 필요하고 기존 note의 제목과 내용을 띄워주기 위해 note를 props로 전달해주도록 하겠습니다. 이후 수정이 완료되면 `modifying`이 변경되고 `Edit`을 띄웠던 `editing`변수도 변경해주기 위해 변경함수도 전달받도록 하겠습니다.

```javascript
function Edit({ addNote, modifyNote, modifying, setEditing, setModifying, note }) {
    const [title, setTitle] = useState(modifying ? note.title : '');
    const [content, setContent] = useState(modifying ? note.content : '');
    const submitHandler = e => {
        e.preventDefault();
        if (modifying) {
            modifyNote(note.id, title, content);
            setModifying(false);
        } else {
            addNote(title, content);
        }
        setEditing(false);
    };
}
```

이제 note값을 전달해주어야 하는데 수정 버튼을 누를 당시 Note컴포넌트로 전달된 `note[selected]`값을 전달해주도록 하겠습니다.

```jsx
function Main({ editing, setEditing }) {
    const [modifying, setModifying] = useState(false);
    return (
        <div>
            <List notes={notes} selectNote={selectNote} deleteNote={deleteNote} />
            {editing ? (
                <Edit
                    addNote={addNote}
                    modifyNote={modifyNote}
                    modifying={modifying}
                    setModifying={setModifying}
                    setEditing={setEditing}
                    note={notes[selected]}
                />
            ) : (
                <Note note={notes[selected]} setEditing={setEditing} />
            )}
        </div>
    );
}
```

### Note

수정하기 버튼을 누를 시 editing이 시작되고 modifying이 시작되도록 전달하고 연결해주도록 하겠습니다.

```jsx
function Note({ note, setEditing, setModifying }) {
    return (
        <div>
            <h2>{note.title}</h2>
            <p>{note.content}</p>
            <button
                onClick={() => {
                    setEditing(true);
                    setModifying(true);
                }}
            >
                수정
            </button>
        </div>
    );
}
```

Main에서는 Note 컴포넌트에게 setModifying도 추가로 전달해주도록 합니다.

```jsx
<Note note={notes[selected]} setEditing={setEditing} setModifying={setModifying} />
```

### List

List에 전달한 notes들을 보여주고 note를 보여주는 Item 컴포넌트에 선택과 삭제할 수 있는 함수를 전달하도록 하겠습니다. 목록을 표현하기 위해 map 함수를 이용해 작성해줍니다. 이때 map으로 인해 같은 컴포넌트가 여러 개 나열될 경우 각 컴포넌트를 리액트가 구분할 수 있도록 key를 props로 전달해주어야 합니다. 이 key는 고유한 값을 가지며 해당 컴포넌트에서는 실제로 전달받을 수 없습니다.

```jsx
function List({ notes, selectNote, deleteNote }) {
    const noteList = notes.map(note => (
        <Item key={note.id} note={note} selectNote={selectNote} deleteNote={deleteNote} />
    ));
    return <ul>{noteList}</ul>;
}
```

### Item

마지막으로 item에 note를 전달받고 선택과 삭제를 연결해주도록 하겠습니다.

```jsx
function Item({ note, selectNote, deleteNote }) {
    return (
        <div onClick={() => selectNote(note.id)}>
            <h3>{note.title}</h3>
            <button onClick={() => deleteNote(note.id)}>삭제</button>
        </div>
    );
}
```
