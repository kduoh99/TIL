## 양식(Forms)

[Vue - 완벽 가이드 (Router 및 Composition API 포함)](https://www.udemy.com/course/vue-router-composition-api/?couponCode=ST12MT030524)

<br/>

**v-model 디렉티브**

Vue.js에서 양식을 다룰 때는 v-model 디렉티브를 사용하여 데이터와 양방향으로 바인딩한다.<br/>
v-model 디렉티브를 이용한 양방향 데이터 바인딩은 select, checkbox, radio button 등에 적용할 수 있다.

- select: 드롭다운 목록에서 하나의 옵션을 선택하는 경우 사용된다.

    ```html
    <div class="form-control">
        <label for="referrer">How did you hear about us?</label>
        <select id="referrer" name="referrer" v-model="referrer">
            <option value="google">Google</option>
            <option value="wom">Word of mouth</option>
            <option value="newspaper">Newspaper</option>
        </select>
    </div>
    ```

- checkbox: 여러 항목을 선택할 수 있는 경우 사용된다.

    ```html
    <div class="form-control">
        <h2>What are you interested in?</h2>
        <div>
            <input id="interest-news" name="interest" type="checkbox" value="news" v-model="interest" />
            <label for="interest-news">News</label>
        </div>
        <div>
            <input id="interest-tutorials" name="interest" type="checkbox" value="tutorials" v-model="interest" />
            <label for="interest-tutorials">Tutorials</label>
        </div>
        <div>
            <input id="interest-nothing" name="interest" type="checkbox" value="nothing" v-model="interest" />
            <label for="interest-nothing">Nothing</label>
        </div>
    </div>
    ```

- radio button: 여러 옵션 중 하나를 선택하는 경우 사용된다.

    ```html
    <div class="form-control">
        <h2>How do you learn?</h2>
        <div>
            <input id="how-video" name="how" type="radio" value="video" v-model="how" />
            <label for="how-video">Video Courses</label>
        </div>
        <div>
            <input id="how-blogs" name="how" type="radio" value="blogs" v-model="how" />
            <label for="how-blogs">Blogs</label>
        </div>
        <div>
            <input id="how-other" name="how" type="radio" value="other" v-model="how" />
            <label for="how-other">Other</label>
        </div>
    </div>
    ```

<br/>

v-model 디렉티브는 몇 가지 수식어를 지원한다.<br/>
이는 디렉티브에 특별한 기능을 추가하는 Vue.js 디렉티브의 문법 요소이다.

  - lazy: 입력 폼에서 다른 요소로 포커스가 이동하는 이벤트가 발생할 때, 입력한 값을 데이터와 동기화한다.
    ```html
    <input type="text" v-model.lazy="name" />
    ```
  - number: 숫자가 입력될 경우, number 타입의 값으로 자동 형변환되어 데이터 옵션 값으로 반영된다.
    ```html
    <input type="text" v-model.number="num" />
    ```
  - trim: 문자열의 앞뒤 공백을 자동으로 제거한다.
    ```html
    <input type="text" v-model.trim="message" />
    ```

<br/>

**blur 이벤트**

- @blur: 입력 데이터가 포커스를 잃는 경우를 감지해서 저장된 값이 유효한지 확인할 수 있다.

    ```html
    <div class="form-control" :class="{invalid: userNameValidity === 'invalid'}">
        <label for="user-name">Your Name</label>
        <input id="user-name" name="user-name" type="text" v-model.trim="userName" @blur="validateInput" />
        <p v-if="userNameValidity === 'invalid'">Please enter a valid name!</p>
    </div>
    ```

<br/>

**커스텀 컴포넌트**

- 'modelValue' props를 통해 외부에서 데이터를 받아오고, 'update:modelValue' 이벤트를 통해 변경된 데이터를 외부로 보낸다.

    ```vue
    <!-- TheForm.vue -->
    <rating-control v-model="rating"></rating-control>
    ```

    ```vue
    <!-- RatingControl.vue -->
    <template>
        <ul>
            <li :class="{active: modelValue === 'poor'}">
                <button type="button" @click="activate('poor')">Poor</button>
            </li>
            <li :class="{active: modelValue === 'average'}">
                <button type="button" @click="activate('average')">Average</button>
            </li>
            <li :class="{active: modelValue === 'great'}">
                <button type="button" @click="activate('great')">Great</button>
            </li>
        </ul>
    </template>

    <script>
    export default {
        props: ['modelValue'],
        emits: ['update:modelValue'],

        methods: {
            activate(option) {
            this.$emit('update:modelValue', option);
            },
        },
    };
    </script>
    ```