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

- img 폴더에 배경으로 사용할 사진 저장

  - 편의상 `0.jpg, 1.jpg, 2.jpg`로 이름 지정

- 랜덤한 번호를 얻어서 body의 배경 이미지로 지정

## background.js

1. images 배열 생성

   - img 폴더 안에 있는 이미지 이름과 동일하게 지정

2. 랜덤 이미지 고르기

   - quotes의 랜덤 로직과 동일

3. html에 이미지 추가

   - javascript에서 이미지 태그 생성

   - img 태그의 src 속성 추가

   - 생성된 태그를 body에 추가

```javascript
const bgImage = document.createElement("img");
bgImage.src = `img/${chosenImage}`;
```

---

### 배경 이미지로 지정하기

- 위와 같은 방법을 사용하면 이미지가 이전에 작성한 택스트 아래에 위치하게 된다.

- 배경 이미지로 지정하고 싶다면 다음 방법 중 하나를 선택한다.

1. img 태그 css 수정하기

- img 태그의 크기와 위치를 수정하여 배경이미지로 사용한다.

```css
img {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: -1;
  opacity: 0.8;
}
```

2. body의 background-image 속성으로 지정하기

- js에서 bgImage를 생성하는 것이 아니라, body 태그의 `backgroundImage` 속성으로 지정한다.

```js
document.body.style.backgroundImage = `url(img/${chosenImage})`;
```

- css도 적절하게 수정하여 이미지 위치를 조절한다.

```css
body {
  background-position: top;
  background-repeat: no-repeat;
  background-size: cover;
  background-attachment: fixed;
}
```

> background-repeat 이미지 반복 여부
> no-repeat: 고정된 이미지 한 개 사용
> repeat(default): 이미지 한 개를 반복해서 패턴으로 사용

> background-attachment 배경 이미지 스크롤 여부
> scroll(default): 선택한 요소와 같이 움직인다.
> fixed: 움직이지 않는다.

- 그러나 body에 배경 이미지를 넣으면 투명도를 조절할 수 없다. body 태그의 투명도를 조절 할 경우, 이미지를 비롯한 모든 자식 태그에 투명도가 지정된다.

- 이미지만 opacity를 조절하기 위해 가상요소를 사용한다. 아쉽게도 pseudo 요소는 javascript DOM요소가 아니라 접근 불가능하다.

- css만으로 배경 이미지를 조절하는 방법을 알아보자.

1. 가상요소(before)를 만들고, 가상 요소에 배경이미지를 넣는다.

2. 기존 박스는 relative, 가상요소는 absolute를 설정한다.

3. 가상요소의 위치는 모두 0을 맞춘다.

4. 다른 자식, 자손은 relative로 설정하여 배경 이미지보다 위로 올라오게 한다.

```css
body::before {
  content: "";
  position: absolute;
  background-image: url(/img/0.jpg);
  background-position: top;
  background-repeat: no-repeat;
  background-size: cover;
  background-attachment: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  opacity: 0.5;
}

body * {
  position: relative;
}
```
