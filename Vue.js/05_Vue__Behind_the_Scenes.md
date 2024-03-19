## Vue: 내부 들여다보기

[Vue - 완벽 가이드 (Router 및 Composition API 포함)](https://www.udemy.com/course/vue-router-composition-api/?couponCode=ST12MT030524)

<br/>

**Proxy**

- 객체의 속성을 읽거나 작업을 가로채기 위해 래핑할 수 있는 객체로, Vue 3에서는 내부적으로 Proxy를 사용한다.
- Vue 인스턴스를 생성하면 data로 지정된 객체를 자동으로 Proxy 객체로 래핑하여 반응성을 제공한다.

    ```javascript
    const data = {
        message: 'Hello!',
        longMessage: 'Hello! World!'
    };

    const handler = {
        set(target, key, value) {
            if (key === 'message') {
                target.longMessage = value + ' World!';
            }
            target.message = value;
        }
    };

    const proxy = new Proxy(data, handler);

    proxy.message = 'Hello!!!!';

    console.log(proxy.longMessage);
    ```

<br/>

**ref**

- Vue에서 제공하는 기능으로, 필요할 때만 DOM 요소로부터 값을 가져온다.
- 템플릿에서 설정한 ref 식별자를 사용하여 Vue 인스턴스 내에서 해당 DOM 요소에 접근할 수 있다.
- 기본 타입의 값을 이용해 반응성을 가진 참조형 데이터를 생성할 때 사용한다.

    ```html
    <section id="app">
        <h2>How Vue Works</h2>
        <input type="text" ref="userText">
        <button @click="setText">Set Text</button>
        <p>{{ message }}</p>
    </section>
    ```

    <br/>

    ```javascript
    setText() {
        this.message = this.$refs.userText.value;
    }
    ```

<br/>

**Vue가 DOM을 업데이트하는 방법**

- Vue는 가상 DOM을 사용하여 작업을 수행한다.
  이는 실제 DOM의 가상 복사본으로, JavaScript가 관리하며 메모리에 위치한다.
  데이터가 변경되면 Vue는 새로운 가상 DOM을 생성하여 기존의 가상 DOM과 비교하고, 두 가상 DOM 간의 차이점만 실제 DOM에 적용함으로써 효율적으로 업데이트를 수행한다.

<br/>

**Vue 앱 생명주기**

- [Vue 3의 생명주기](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)
- Vue 컴포넌트의 생성부터 제거까지의 흐름을 말하며, 각 생명주기마다 실행할 수 있는 이벤트 훅을 등록할 수 있다.

    ```javascript
    const app = Vue.createApp({
        data() {
            return {
                currentUserInput: '',
                message: 'Vue is great!',
            };
        },
        methods: {
            saveInput(event) {
                this.currentUserInput = event.target.value;
            },
            setText() {
                this.message = this.$refs.userText.value;
            },
        },
        beforeCreate() {
            console.log('beforeCreate()');
        },
        created() {
            console.log('created()');
        },
        beforeMount() {
            console.log('beforeMount()');
        },
        mounted() {
            console.log('mounted()');
        },
        beforeUpdate() {
            console.log('beforeUpdate()');
        },
        update() {
            console.log('updated()');
        },
        beforeUnmount() {
            console.log('beforeUnmount()');
        },
        unmounted() {
            console.log('unmounted()');
        },
    });

    app.mount('#app');

    setTimeout(function () {
        app.unmount();
    }, 3000);
    ```