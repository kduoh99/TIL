## 라우팅(Routing): "다중 페이지"가 있는 싱글 페이지 애플리케이션(SPA) 구축하기

[Vue - 완벽 가이드 (Router 및 Composition API 포함)](https://www.udemy.com/course/vue-router-composition-api/?couponCode=ST12MT030524)

<br/>

Vue.js에서 싱글 페이지 애플리케이션(SPA)의 페이지 이동은 Vue Router 라이브러리를 통해 구현된다. 이 과정에서 페이지 전체를 새로고침하지 않고, 필요한 뷰나 컴포넌트를 동적으로 렌더링하여 사용자 경험을 향상시킨다.

<br/>

**Vue Router 설치 및 적용**

  ```bash
  npm install vue-router
  ```

  ```javascript
  // main.js
  import { createApp } from 'vue';
  import { createRouter, createWebHistory } from 'vue-router';

  import App from './App.vue';
  import TeamsList from './components/teams/TeamsList.vue';
  import UsersList from './components/users/UsersList.vue';

  // 라우터 인스턴스 생성 및 설정
  const router = createRouter({
    history: createWebHistory(), // HTML5 History 모드 사용
    routes: [
      { path: '/teams', component: TeamsList },
      { path: '/users', component: UsersList },
    ]
  });

  // Vue 앱 생성 및 라우터 적용
  const app = createApp(App);
  app.use(router);
  app.mount('#app');
  ```

<br/>

**동적 라우트**
  
  URL의 일부가 변할 수 있는 값을 포함할 때 사용한다.

  ```javascript
  {
    path: '/teams/:teamId',
    component: TeamMembers,
    props: true
  }
  ```

<br/>

**중첩 라우트**
  
  부모-자식 컴포넌트 관계를 URL 경로에 반영하여 구성한다.

  ```javascript
  {
    path: '/teams',
    component: TeamsList,
    children: [
      { path: ':teamId', component: TeamMembers, props: true }
    ]
  }
  ```

<br/>

**명명된 라우터 및 뷰**

  명명된 라우트와 뷰를 사용하여 더 유연하고 관리하기 쉬운 라우트 구성을 만들 수 있다.

  ```javascript
  {
    name: 'teams',  // 라우트에 이름을 지정
    path: '/teams',
    components: { 
      default: TeamsList, // 기본 컴포넌트
      footer: TeamsFooter // 명명된 뷰 'footer'에 해당하는 컴포넌트
    }
  }
  ```

<br/>

**스크롤 동작 제어**
  
  scrollBehavior 함수를 사용하여 라우트 이동 시 스크롤 위치를 제어할 수 있다.

  ```javascript
  scrollBehavior(to, from, savedPosition) {
    // 라우트 이동 시 스크롤 위치 제어
    if (savedPosition) {
      return savedPosition;
    }
    return { left: 0, top: 0 };
  }
  ```

<br/>

**내비게이션 가드**

  내비게이션 가드를 사용하여 라우트 이동 시 특정 조건을 만족하는 경우에만 이동하도록 제어한다.

  ```javascript
  // 전역 가드
  router.beforeEach(function(to, from, next) {
    // 라우트 변경 전 로직
    next(); // 다음 라우트로 이동
  });

  router.afterEach(function(to, from) {
    // 라우트 변경 후 로직
  });
  ```

<br/>

**내비게이션 방법**

  - 프로그래밍 방식
  
    JavaScript 코드를 사용하여 페이지 이동을 제어한다.

    ```javascript
    this.$router.push('/teams');  // 지정된 경로로 이동
    this.$router.replace('/users'); // 현재 페이지를 새로운 페이지로 대체
    ```

  - 선언적 방식
  
    `<router-link>` 태그를 사용하여 HTML 템플릿 내에서 페이지 이동을 선언한다.

    ```html
    <router-link to="/teams">Teams</router-link>
    ```