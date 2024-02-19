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

- todo를 저장하는 배열을 만들고, todo가 생성될 때 배열에 추가한다.

```javascript
const toDos = [];

function handleToDoSubmit(e) {
  e.preventDefault();
  const newTodo = toDoInput.value;
  toDoInput.value = "";
  // 배열에 추가
  toDos.push(newTodo);
  paintToDo(newTodo);
}
```

- localStorage에 저장하기

  ```javascript
  function saveToDos() {
    localStorage.setItem("todos", toDos);
  }

  function handleToDoSubmit(e) {
    ...
    saveToDos();
  }
  ```

  - localStorage에는 string만 저장할 수 있다. 위와 같이 저장하게 되면, todos는 "a, b, c"와 같이 저장된다.

  - array 형태로 저장하기 위해 javascript의 `JSON.stringify()`를 사용한다.

  > JSON.stringify(value)
  > javascript 값이나 객체를 JSON 문자열로 변환한다.

  - 따라서 다음과 같이 수정한다.

  ```javascript
  function saveToDos() {
    localStorage.setItem("todos", JSON.stringify(toDos));
  }
  ```

- localStorage 값 불러오기

  > JSON.parse(text)
  > 문자열을 JSON 구문으로 변환한다.

  - localStorage의 string 값으로 저장된 데이터를 배열로 받아온다.

  - `forEach()` 메서드를 사용해서 각 요소를 화면에 그린다.

  > Array.forEach(callbackFn)
  > Array의 각 배열 요소에 대해 제공된 함수를 한 번씩 실행한다.
  >
  > callbackFn은 다음 인수를 사용하여 호출된다
  > `element`: 배열에서 현재 처리 중인 요소
  > `index`: 배열에서 처리 중인 현재 요소의 인덱스
  > `array`: `forEach()`에서 호출된 배열
  >
  > `forEach()`는 루프를 중지하거나 중단할 수 없다.

  ```javascript
  const TODOS_KEY = "todos";

  const savedToDos = localStorage.getItem(TODOS_KEY);

  if (savedToDos) {
    const parseToDos = JSON.parse(savedToDos);
    parseToDos.forEach((item) => {
      paintToDo(item);
    });
    // 아래 표현도 정상적으로 작동한다.
    // parseToDos.forEach(paintToDo);
  }
  ```

- localStorage 덮어쓰기

  - 정상적으로 작동하는 것처럼 보이지만, todo를 새로 작성하면 localStorage가 비워지고 새로 추가된다.

  - 이는 새로고침 시 로드된 js에서 toDos가 빈 배열로 초기화되고 있기 때문이다. 다음과 같이 toDos의 초깃값을 변경한다.

  ```javascript
  let toDos = [];

  if (savedToDos) {
    const parseToDos = JSON.parse(savedToDos);
    toDos = parseToDos;
    parseToDos.forEach(paintToDo);
  }
  ```

- 삭제 로직 수정

  - 이제 localStorage에서 toDos를 관리하기 때문에 localStorage에서 삭제해야 한다.

  - 이전에 작성한 deleteToDO 함수는 지워야 할 HTML의 element 태그를 선택해 지우고 있다. 그러나, 데이터베이스의 경우에는 지워야 할 태그를 알 지 못한다.

  - 이를 위해 todo를 id를 가진 object 형태로 저장한다. 랜덤한 id는 `Date.now()`를 사용한다.

  ```javascript
  function handleToDoSubmit(e) {
    ...
    const newTodoObj = {
      text: newTodo,
      id: Date.now(),
    };
    toDos.push(newTodoObj);
    paintToDo(newTodoObj);
    ...
  }
  ```

  - li태그에 id 속성을 추가하고, 저장된 object의 text를 화면에 그린다.

  ```javascript
  function paintToDo(newToDo) {
    const li = document.createElement("li");
    li.id = newToDo.id;
    ...
    // before
    span.innerText = newToDo;
    // after
    span.innerText = newToDo.text;
    ...
  }
  ```

  - 지우고 싶은 todo의 id를 얻고, 해당 id의 값을 제외한 array로 todos를 업데이트한다. JS의 `filter()`을 사용할 수 있다.

  > array.filter(callbackFn)
  > 주어진 배열의 얕은 복사본을 생성하고, 제공된 함수에 의해 구현된 테스트를 통과하는 배열의 요소만 필터링한다.
  >
  > 테스트를 통과한 요소만 포함하는 새 배열을 반환한다. 테스트를 통과한 요소가 없다면 빈 배열이 반환된다.
  >
  > callbackFn에서 배열의 요소를 유지하려면 true를, 그렇지 않으면 false를 반환해야 한다.
  > callbackFn의 인수는 forEach의 callbackFn과 같다.

  - `li.id`는 string 타입, `toDo.id`는 number 타입이기 떄문에, 형변환을 해줘야 한다.

  - toDos 배열에 변동이 생겼으므로, localStorage 값을 업데이트 해준다.

  ```javascript
  function deleteToDo(event) {
    const li = event.target.parentElement;
    li.remove();
    toDos = toDos.filter((toDo) => toDo.id !== parseInt(li.id));
    saveToDos();
  }
  ```
