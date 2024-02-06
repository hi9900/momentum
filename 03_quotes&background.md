# Quotes

1. 명언과 이름을 담을 태그 생성

```html
<div id="quote">
  <span></span>
  <span></span>
</div>
```

```js
const quote = document.querySelector("quote span:first-child");
const author = document.querySelector("quote span:last-child");
```

2. 명언 리스트를 랜덤으로 출력

- Math 모듈을 사용

  - `Math.random()`: 0 ~ 1 사이의 난수 생성

  - `Math.random() * 10`: 0 ~ 10 사이의 난수 생성

  - `Math.round(N)`: N을 반올림

  - `Math.ceil(N)`: N을 올림

  - `Mate.floor(N)`: N을 내림

- `Math.floor(Math.random() * 10)`

  - 명언 리스트가 10일 때 0 ~ 9 사이의 값을 가져올 수 있음

- `Math.floor(Math.random() * quotes.length)`

  - 0 ~ (명언 리스트의 길이 - 1) 사이의 값을 가져옴

```js
const todaysQuote = quotes[Math.floor(Math.random() * quotes.length)];

quote.innerText = todaysQuote.quote;
author.innerText = todaysQuote.author;
```

---

# Background
