## CLOCK

1. 시계를 표시할 태그 작성 및 default Text 설정

```html
<h2 id="clock">00:00:00</h2>
```

- js 파일 내에서 시계 태그 선택

```js
const clock = document.querySelector("h2#clock");
```

2. interval

> interval
>
> 매번 일어나야 하는 것
> ex. 매 2초마다 무슨 일이 일어나게 하고 싶을 때 사용

`setInterval(f, time)`

- f: 실행할 함수

- time: 실행 간격, ms(milliseconds)

3. timeout

> 함수를 일정 시간이 흐른 뒤 한 번만 호출

`setTimeout(f, time)`

- f: 실행할 함수

- time: 실행 시작 시간, ms(milliseconds)

```js
function sayHello() {
  console.log("Hello!");
}

// 5초 마다 실행
setInterval(sayHello, 5000);

// 5초를 기다렸다가 한 번 실행
setTimeout(sayHello, 5000);
```

4. Date

- 1초 마다 시간을 가져오기

```js
function getClock() {
  const date = new Date();
  clock.innerText = `${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`;
}

setInterval(getClock, 1000);
```

- 매 초 새로운 new Date() object를 가져오고 있다.

- new Date는 현재 날짜, 시간, 분, 초에 대한 정보를 가지고 있고, 이것을 매 초 가져오면서 시계처럼 보인다.

- setInterval 함수는 특정 시간 뒤에 실행된다. website가 load 될 때 함수를 실행하고 매 초 다시 실행되게 하기 위해 한 번 호출 한다.

```js
getClock();
setInterval(getClock, 1000);
```

5. PadStart

- 한 자리 숫자를 0을 포함한 두 자리 숫자로 표기

- string이 적어도 2개의 문자를 가지고 있게 하기 위해 해당 함수를 사용한다.

`string.padStart(N, a)`

- string의 길이가 N이 아니라면, 해당 string의 앞에 a를 채워 길이 N을 맞춘다.

`string.padEnd(N, a)`

- string의 길이가 N이 아니라면, 해당 string의 뒤에 a를 채워 길이 N을 맞춘다.

```js
function getClock() {
  const date = new Date();
  const hours = String(date.getHours()).padStart(2, "0");
  const minutes = String(date.getMinutes()).padStart(2, "0");
  const seconds = String(date.getSeconds()).padStart(2, "0");
  clock.innerText = `${hours}:${minutes}:${seconds}`;
}
```

- `date.getHours(), ...`은 number이므로 `String()`을 사용해 문자열로 바꿔준다.

- 문자열 함수인 `padStart()`를 사용해 2자리로 표기한다.

---

### 전체 시계 코드

```js
const clock = document.querySelector("h2#clock");

function getClock() {
  const date = new Date();
  const hours = String(date.getHours()).padStart(2, "0");
  const minutes = String(date.getMinutes()).padStart(2, "0");
  const seconds = String(date.getSeconds()).padStart(2, "0");
  clock.innerText = `${hours}:${minutes}:${seconds}`;
}

getClock();
setInterval(getClock, 1000);
```

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="css/style.css" />
  </head>

  <body>
    <form id="login-form" class="hidden">
      <input
        required
        maxlength="15"
        type="text"
        placeholder="이름을 입력하세요"
      />
      <button>로그인</button>
    </form>
    <h2 id="clock">00:00:00</h2>
    <h1 id="greeting" class="hidden"></h1>
    <script src="js/greeting.js"></script>
    <script src="js/clock.js"></script>
  </body>
</html>
```
