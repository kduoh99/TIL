## 애니메이션 및 전환

[Vue - 완벽 가이드 (Router 및 Composition API 포함)](https://www.udemy.com/course/vue-router-composition-api/?couponCode=ST12MT030524)

<br/>

**애니메이션 기초 및 CSS 전환**

CSS의 `transition` 속성을 사용하면 요소의 상태 변화를 부드럽게 표현할 수 있다.
  
```css
/* 요소의 속성, 지속 시간, 타이밍 함수를 설정 */
.block {
    transition: transform 0.3s ease-out;
}
/* 해당 요소의 위치가 왼쪽으로 150px 이동 */
.animate {
    transform: translateX(-150px);
}
```

<br/>

`@keyframes` 를 사용하여 각 프레임별로 스타일을 정의하고 애니메이션을 제어할 수 있다.
  
```css
/* 애니메이션 효과를 정의하는 keyframes 설정 */
.animate {
    animation: slide-fade 0.3s ease-out forwards;
}
  
/* 각 프레임의 스타일을 설정하고, 시간에 따라 애니메이션을 제어 */
@keyframes slide-fade {
    0% {
        transform: translateX(0) scale(1);
    }
    
    70% {
        transform: translateX(-120px) scale(1.1);
    }
    
    100% {
        transform: translateX(-150px) scale(1);
    }
}
```

<br/>

**Vue 전환 컴포넌트가 있는 CSS 애니메이션**

Vue.js에서는 요소의 추가 및 제거 시, CSS 애니메이션을 자동으로 처리할 수 있다. 이는 Vue의 전환(transition) 컴포넌트를 활용하여 구현할 수 있다.

```vue
<template>
    <!-- 모달 애니메이션을 위한 Vue 전환 컴포넌트 -->
    <transition name="modal">
        <!-- open 속성을 통해 모달의 표시 여부를 제어 -->
        <dialog open v-if="open">
            <!-- 모달 내용을 삽입할 슬롯 -->
            <slot></slot>
        </dialog>
    </transition>
</template>

<style scoped>
/* 모달 입장 및 퇴장 애니메이션 */
.modal-enter-active {
    animation: modal 0.3s ease-out;
}

.modal-leave-active {
    animation: modal 0.3s ease-in reverse;
}

/* 모달의 애니메이션 키프레임 설정 */
@keyframes modal {
    from {
        opacity: 0; /* 시작 상태의 투명도 */
        transform: translateY(-50px) scale(0.9);  /* 시작 상태의 변형(transform) */
  }
  
    to {
        opacity: 1; /* 종료 상태의 투명도 */
        transform: translateY(0) scale(1);  /* 종료 상태의 변형(transform) */
    }
}
</style>
```

<br/>

**JavaScript 전환 구축**

Vue 트랜잭션 이벤트를 처리하기 위한 메서드 (`beforeEnter`, `enter`, `beforeLeave`, `leave`)를 활용하여 요소의 상태 변화에 따른 애니메이션 효과를 조절한다.

- `setInterval`
  
  일정한 시간 간격으로 코드 블록을 반복 실행하는 JavaScript 함수이다.
  애니메이션을 구현할 때 요소의 스타일을 일정 간격으로 변경하는데 사용한다.

- `clearInterval`
  
  setInterval에 의해 설정된 일정한 시간 간격으로 반복 실행되는 코드 블록을 중지하는 JavaScript 함수이다. 애니메이션 종료 시에 사용되어 애니메이션을 중지하고 완료 콜백을 호출한다.
  
- `el`
  
  Vue 트랜지션에서 애니메이션을 적용할 대상 요소를 가리키는 매개변수이다.

- `done`
  
  애니메이션 완료를 알리는 콜백 함수로, 애니메이션 처리가 완료되면 호출하여 Vue에게 애니메이션이 완료되었음을 알린다.

```javascript
methods: {
    // 들어가는 애니메이션 취소될 때 처리
    enterCancelled(el) {
        clearInterval(this.enterInterval);
    },
    // 나가는 애니메이션 취소될 때 처리
    leaveCancelled(el) {
        clearInterval(this.leaveInterval);
    },
    // 요소가 들어가기 전에 초기 상태 설정
    beforeEnter(el) {
        el.style.opacity = 0;
    },
    // 요소가 들어갈 때의 애니메이션 처리
    enter(el, done) {
        let round = 1;
        this.enterInterval = setInterval(() => {
            el.style.opacity = round * 0.01;
            round++;
            if (round > 100) {
                clearInterval(this.enterInterval);
                done();
            }
        }, 20);
    },
    // 요소가 나가기 전에 초기 상태 설정
    beforeLeave(el) {
        el.style.opacity = 1;
    },
    // 요소가 나갈 때의 애니메이션 처리
    leave(el, done) {
        let round = 1;
        this.leaveInterval = setInterval(() => {
            el.style.opacity = 1 - round * 0.01;
            round++;
            if (round > 100) {
                clearInterval(this.leaveInterval);
                done();
            }
        }, 20);
    },
}
```

<br/>

**목록에 애니메이션 적용**

`transition-group` 컴포넌트를 사용하여 여러 요소에 동시에 애니메이션을 적용할 수 있다.<br/>
이는 실제 DOM 요소를 렌더링하며, `tag` 속성으로 렌더링될 요소를 지정한다.

```html
<!-- transition-group 컴포넌트를 사용하여 사용자 목록을 감싸고 애니메이션 적용 -->
<transition-group tag="ul" name="user-list">
    <li v-for="user in users" :key="user" @click="removeUser(user)">{{ user }}</li>
</transition-group>
```

여러 항목에 애니메이션을 적용하고 싶다면 `transition-group` 컴포넌트를, 애니메이션을 적용할 항목이 하나 혹은 서로 교체되는 두 항목일 경우에는 `transition`을 사용하면 된다.

<br/>

**라우트 변경에 애니메이션 적용**

Vue 3에서는 `router-view` 내부에 `transition` 을 위치시키고, 활성화된 라우트에 따라 렌더링될 컴포넌트가 제어된다. 이때 컴포넌트는 정확히 하나의 루트 요소를 가져야 한다.

```vue
<!-- App.vue -->
<template>
  <!-- 현재 라우트의 컴포넌트를 표시하는 <router-view> -->
  <router-view v-slot="slotProps">
    <!-- 페이지 전환 시 애니메이션을 적용하는 <transition> -->
    <transition name="fade-button" mode="out-in">
      <!-- 현재 라우트에 해당하는 컴포넌트를 동적으로 표시 -->
      <component :is="slotProps.Component"></component>
    </transition>
  </router-view>
</template>
```

```javascript
import { createApp } from 'vue';
import { createRouter, createWebHistory } from 'vue-router';

import App from './App.vue';
import BaseModal from './components/BaseModal.vue';
import AllUsers from './pages/AllUsers.vue';
import CourseGoals from './pages/CourseGoals.vue';

// Vue Router 설정
const router = createRouter({
    history: createWebHistory(),
    routes: [
        { path: '/', component: AllUsers },      // '/' 경로에는 AllUsers 컴포넌트를 연결
        { path: '/goals', component: CourseGoals }  // '/goals' 경로에는 CourseGoals 컴포넌트를 연결
    ]
});

const app = createApp(App);

app.component('base-modal', BaseModal);

app.use(router);

// Vue Router가 초기화되고 준비되면 앱을 마운트
router.isReady().then(function() {
    app.mount('#app');
});
```

```vue
<!-- AllUsers.vue -->
<template>
    <div class="container">
        <h2>All Users</h2>
        <router-link to="/goals">Course Goals</router-link>
    </div>
</template>
```

```vue
<!-- CourseGoals.vue -->
<template>
    <div class="container">
        <h2>Course Goals</h2>
        <router-link to="/">All Users</router-link>
    </div>
</template>
```