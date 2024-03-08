## 기초 및 핵심 개념 - Vue를 이용한 DOM 상호작용

[Vue - 완벽 가이드 (Router 및 Composition API 포함)](https://www.udemy.com/course/vue-router-composition-api/?couponCode=ST12MT030524)

<br/>

### Vue 앱 연결 및 제어

- Vue 앱은 하나의 HTML 요소에만 연결할 수 있으며, 이를 위해 ID 선택자나 고유한 CSS 선택자를 사용한다.<br/>
- Vue로 HTML 요소를 제어할 때, 해당 요소의 자식 요소도 제어할 수 있다.<br/><br/>

### 보간법

- '{{ }}'의 형태를 가지며, 콧수염 표현식이라고도 불린다.<br/>
- 문자열을 템플릿에 덧붙여 동적으로 데이터를 표시하는 방식을 말한다.<br/><br/>

### v-bind 디렉티브

- 요소의 속성을 바인딩하기 위해 사용하며, 단방향으로만 데이터 바인딩을 수행한다.<br/><br/>

### 메서드

- Vue 인스턴스에서 사용할 메서드를 등록하는 옵션이다.<br/>
- 등록된 메서드는 Vue 인스턴스를 이용해 직접 호출할 수 있으며, 디렉티브 표현식, 콧수염 표현식, 이벤트 핸들러로도 이용할 수 있다.<br/><br/>

### 이벤트 처리

- Vue의 이벤트 처리는 HTML, 자바스크립트에서 사용하는 이벤트를 준용해서 사용하므로 HTML, 자바스크립트의 이벤트 처리 방법을 익혀두면 더욱 도움이 될 것이다.<br/>
  - [HTML](https://developer.mozilla.org/en-US/docs/Web/Events)<br/>
  - [자바스크립트](https://www.w3schools.com/tags/ref_eventattributes.asp)<br/>
- 이벤트 처리는 'v-on' 디렉티브를 이용할 수 있으며, 'v-on:[이벤트명]' 혹은 '@이벤트명'으로 사용한다.<br/><br/>

### 실습: 데이터 바인딩

![실습: 데이터 바인딩](./img/assignment_1.png)<br/>

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue Basics</title>
    <link
      href="https://fonts.googleapis.com/css2?family=Jost:wght@400;700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="styles.css" />
    <script src="https://unpkg.com/vue@3" defer></script>
    <script src="app.js" defer></script>
  </head>
  <body>
    <section id="assignment">
      <!-- 1) Output your name -->
      <h2>{{ name }}</h2>
      <!-- 2) Output your age -->
      <p>{{ age }}</p>
      <!-- 3) Output your age + 5 -->
      <p>{{ calAge() }} in 5 years</p>
      <!-- 4) Output a random number (0 to 1) -->
      <p>Favorite Number: {{ randomNumber() }}</p>
      <div>
        <!-- 5) Display some image you found via Google -->
        <img src="../img/turtle.JPG" alt="No Image" width="150" />
      </div>
      <!-- 6) Prepopulate the input field with your name via the "value" attribute -->
      <input type="text" :value="name" />
    </section>
  </body>
</html>
```

<br />

```javascript
const app = Vue.createApp({
  data() {
    return { name: "Kang Duoh", age: 26 };
  },
  methods: {
    calAge() {
      return this.age + 5;
    },
    randomNumber() {
      return Math.random();
    },
  },
});
app.mount("#assignment");
```

<br />

```css
* {
  box-sizing: border-box;
}
html {
  font-family: "Jost", sans-serif;
}
body {
  margin: 0;
}
section {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem;
  border-radius: 10px;
  padding: 1rem;
  text-align: center;
}
h2 {
  font-size: 2rem;
  border-bottom: 4px solid #ccc;
  color: #970076;
  margin: 0 0 1rem 0;
}
p {
  font-size: 1.25rem;
  font-weight: bold;
  background-color: #970076;
  padding: 0.5rem;
  color: white;
  border-radius: 25px;
}
input {
  font: inherit;
  border: 1px solid #ccc;
}
input:focus {
  outline: none;
  border-color: #1b995e;
  background-color: #d7fdeb;
}
button {
  font: inherit;
  cursor: pointer;
  border: 1px solid #ff0077;
  background-color: #ff0077;
  color: white;
  padding: 0.05rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
}
button:hover,
button:active {
  background-color: #ec3169;
  border-color: #ec3169;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}
```

<br /><br />

### 실습: 이벤트 바인딩 ![실습: 이벤트 바인딩](./img/assignment_2.png)<br /><br />

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue Basics</title>
    <link
      href="https://fonts.googleapis.com/css2?family=Jost:wght@400;700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="styles.css" />
    <script src="https://unpkg.com/vue@3" defer></script>
    <script src="app.js" defer></script>
  </head>
  <body>
    <header>
      <h1>Events</h1>
    </header>
    <section id="assignment">
      <h2>Event Practice</h2>
      <!-- 1) Show an alert (any text of your choice) when the button is pressed -->
      <button v-on:click="showAlert">Show Alert</button>
      <hr />
      <!-- 2) Register the user input on "keydown" and output it in the paragraph (hint: event.target.value helps) -->
      <input type="text" v-on:keydown="userInput1" />
      <p>{{ output1 }}</p>
      <hr />
      <!-- 3) Repeat 2) but only output the entered value if the ENTER key was pressed -->
      <input
        type="text"
        v-on:keydown="userInput1"
        v-on:keyup.enter="userInput2"
      />
      <p>{{ output2 }}</p>
    </section>
  </body>
</html>
```

<br />

```javascript
const app = Vue.createApp({
  data() {
    return { output1: "", output2: "" };
  },
  methods: {
    showAlert() {
      alert("Warning!");
    },
    userInput1(event) {
      this.output1 = event.target.value;
    },
    userInput2() {
      this.output2 = this.output1;
    },
  },
});
app.mount("#assignment");
```

<br />

```css
* {
  box-sizing: border-box;
}
html {
  font-family: "Jost", sans-serif;
}
body {
  margin: 0;
}
header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem;
  border-radius: 10px;
  padding: 1rem;
  background-color: #1b995e;
  color: white;
  text-align: center;
}
#assignment {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem;
  border-radius: 10px;
  padding: 1rem;
  text-align: center;
}
#assignment h2 {
  font-size: 2rem;
  border-bottom: 4px solid #ccc;
  color: #1b995e;
  margin: 0 0 1rem 0;
}
#assignment p {
  font-size: 1.25rem;
  font-weight: bold;
  background-color: #8ddba4;
  padding: 0.5rem;
  color: #1f1f1f;
  border-radius: 25px;
}
#assignment input {
  font: inherit;
  border: 1px solid #ccc;
}
#assignment input:focus {
  outline: none;
  border-color: #1b995e;
  background-color: #d7fdeb;
}
#assignment button {
  font: inherit;
  cursor: pointer;
  border: 1px solid #ff0077;
  background-color: #ff0077;
  color: white;
  padding: 0.05rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
}
#assignment button:hover,
#assignment button:active {
  background-color: #ec3169;
  border-color: #ec3169;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}
```
