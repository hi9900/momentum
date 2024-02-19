# TO-DO List

1. 할 일을 입력받을 폼 태그, 출력할 리스트 태그 작성

```html
<form id="todo-form">
  <input type="text" placeholder="할 일을 입력하세요" required />
</form>
<ul id="todo-list"></ul>
```

2. JavaScript에 태그를 연결하고, submit function을 만든다.

```javascript
const toDoForm = document.getElementById("todo-form");
const toDoList = document.getElementById("todo-list");

function handleToDoSubmit(e) {
  e.preventDefault();
}

toDoForm.addEventListener("submit", handleToDoSubmit);
```

3. submit 함수는 다음과 같이 동작한다.

- 입력된 할 일을 저장하고, 인풋 태그 안의 내용을 지운다.

```javascript
function handleToDoSubmit(e) {
  e.preventDefault();
  const newTodo = toDoInput.value;
  toDoInput.value = "";
}
```

4. 할 일 입력받아 이를 출력하는 함수 paintToDo를 정의한다.

- li 태그에 나중에 삭제 버튼을 추가할 것이므로, `li > span`에 텍스트를 작성한다.

  1. li 태그와 span 태그를 각각 작성한 후, li의 자식으로 span 태그를 추가한다.

  2. span 태그 내부에 할 일을 입력한다.

  3. toDoList에 li 태그를 추가한다.

```javascript
function paintToDo(newToDo) {
  const li = document.createElement("li");
  const span = document.createElement("span");
  li.appendChild(span);
  span.innerText = newToDo;
  toDoList.appendChild(li);
}
```

- 지금까지의 코드는 todo item을 삭제할 수 없고, 새로고침 시 입력받은 todo가 모두 사라진다.

---

5. TODO 삭제

- `li` 태그 내부에 `button`을 만든다. 삭제를 위한 버튼이므로, "❌"를 표시한다.

- 만들어진 button의 이벤트리스너를 등록한다.

```javascript
function paintToDo(newToDo) {
  const li = document.createElement("li");
  const span = document.createElement("span");
  span.innerText = newToDo;
  const button = document.createElement("button");
  button.innerText = "❌";
  button.addEventListener("click", deleteToDo);
  li.appendChild(span);
  li.appendChild(button);
  toDoList.appendChild(li);
}
```

- todo를 삭제하는 deleteToDo 함수를 작성한다.

  - `event.target`은 click된 element를 보여준다.

  - `event.target.parentNode` 또는 `parentElement`는 부모 요소, 여기서는 `li`태그를 찾는다.

  - `remove` 함수를 사용해서 찾은 li 태그를 삭제할 수 있다.

```javascript
function deleteToDo(event) {
  const li = event.target.parentElement;
  li.remove();
}
```

6. TODO 저장
