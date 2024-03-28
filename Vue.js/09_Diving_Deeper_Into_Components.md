## 컴포넌트 자세히 알아보기

[Vue - 완벽 가이드 (Router 및 Composition API 포함)](https://www.udemy.com/course/vue-router-composition-api/?couponCode=ST12MT030524)

<br/>

**전역 컴포넌트와 지역 컴포넌트**

Vue.js에서 컴포넌트는 재사용 가능한 UI 요소를 의미한다.

- 전역 컴포넌트
  - Vue 애플리케이션 전체에서 사용할 수 있는 컴포넌트이다.
  - 'Vue.component()' 메서드를 사용하여 등록되며, 템플릿에서 해당 컴포넌트를 사용할 때의 태그명과 컴포넌트 객체를 인자로 전달한다.
  - 모든 자식 컴포넌트에서 사용할 수 있으며, 주로 애플리케이션의 여러 위치에서 공통으로 사용되는 경우에 활용된다.

  ```javascript
  import TheHeader from './components/TheHeader.vue'

  const app = createApp(App);
  app.component('the-header', TheHeader);
  app.mount('#app');
  ```
 
- 지역 컴포넌트
  - 특정 컴포넌트 내에서만 사용되는 컴포넌트이다.
  - 컴포넌트 객체에서 'components' 옵션을 사용하여 등록되며, 템플릿에서 사용할 태그명과 컴포넌트 객체를 지정한다.
  - 해당 컴포넌트 내부에서만 사용할 수 있으며, 특정 컴포넌트에서만 필요한 경우에 활용된다.

  ```vue
  <script>
    import TheHeader from './components/TheHeader.vue'
    
    export default {
      components: {
        'the-header': TheHeader
      },
      ...
    }
  </script>
  ```

<br/>

**범위가 지정된 스타일**

- Scoped CSS는 특정 컴포넌트에서만 사용되는 스타일을 정의할 때 충돌을 피하기 위해 사용되며, 'style scoped'로 변경하여 적용할 수 있다.
- 전역 스타일은 모든 HTML 요소에 영향을 주지만, Scoped CSS는 해당 컴포넌트의 템플릿에 있는 HTML 요소에만 영향을 준다.

<br/>

**슬롯**

- 부모 컴포넌트에서 자식 컴포넌트로 템플릿 정보를 전달하는 방법을 제공한다.
- 외부에서 컴포넌트로 삽입될 수 있는 동적 HTML 코드의 플레이스홀더 역할을 한다.
- 한 개 또는 여러 개의 슬롯을 사용할 수 있으며, 여러 슬롯을 사용할 때는 이름을 지정해야 한다.
- 기본 콘텐츠를 제공할 수 있으며, 외부에서 콘텐츠가 제공되지 않을 때에는 기본 콘텐츠가 사용된다.
- 래퍼 컴포넌트의 제약을 해제하여 컴포넌트를 유연하게 구성하는 핵심 기능을 제공한다.

  ```vue
  <!-- 슬롯 : UserInfo.vue --> 
  <template>
    <base-card>
      ...
    </base-card>
  </template>
  ```

  ```vue
  <!-- 슬롯 : BaseCard.vue -->
  <template>
    <div>
      <slot></slot>
    </div>
  </template>
  ```

  ```vue
  <!-- 이름이 있는 슬롯 : UserInfo.vue --> 
  <template>
    <base-card>
      <template v-slot:header>
        ...
      </template>
    </base-card>
  </template>
  ```

  ```vue
  <!-- 이름이 있는 슬롯 : BaseCard.vue -->
  <template>
    <div>
      <header>
        <slot name="header"></slot>
      </header>
    <div>
  </template>
  ```

  ```vue
  <!-- 범위가 지정된 슬롯 : App.vue --> 
  <template>
    <course-goals>
      <template #default="slotProps">
        <h2>{{ slotProps.item }}</h2>
        <p>{{ slotProps['another-prop'] }}</p>
      </template>
    </course-goals>
  </template>
  ```

  ```vue
  <!-- 범위가 지정된 슬롯 : CourseGoals.vue -->
  <template>
    <ul>
      <li v-for="goal in goals" :key="goal">
        <slot :item="goal" another-prop="..."></slot>
      </li>
    </ul>
  </template>
  ```

<br/>

**동적 컴포넌트**

- <component> 요소를 사용하여 템플릿에서 동적으로 컴포넌트를 렌더링할 수 있으며, is 특성을 이용하여 표시할 컴포넌트를 결정한다.
- is 특성에는 등록한 컴포넌트의 이름만 사용할 수 있으며, 다양한 표기법(케밥 표기법, 파스칼 표기법, 카멜 표기법)을 사용할 수 있다.
- 만약 자식 컴포넌트가 정적인 경우, 매번 렌더링하는 것은 효율적이지 않다.
  따라서 <component> 요소를 <keep-alive> 요소로 감싸서 컴포넌트를 캐싱하는 것이 효율적이다.

  ```vue
  <!-- ActiveGoals.vue -->
  <template>
    <h2>ActiveGoals</h2>
  </template>
  ```

  ```vue
  <!-- ManageGoals.vue -->
  <template>
    <div>
      <h2>ManageGoals</h2>
      <input type="text" />
    </div>
  </template>
  ```

  ```vue
  <!-- App.vue -->
  <template>
    <div>
      <button @click="setSelectedComponent('active-goals')">Active Goals</button>
      <button @click="setSelectedComponent('manage-goals')">Manage Goals</button>
      <keep-alive>
        <component :is="selectedComponent"></component>
      <keep-alive>
    </div>
  </template>

  <script>
    import ActiveGoals from './components/ActiveGoals.vue';
    import ManageGoals from './components/ManageGoals.vue';

    export default {
      components: {
        ActiveGoals,
        ManageGoals
      },
      data() {
        return {
          selectedComponent: 'active-goals'
        }
      },
      methods: {
        setSelectedComponent(cmp) {
          this.selectedComponent = cmp;
        }
      }
    };
  </script>
  ```

<br/>

**텔레포트**

- 메인 화면과 독립적이면서 공유 UI를 제공해야 할 때 사용되며, 이를 통해 컴포넌트 트리의 계층 구조와 관계없이 별도의 요소에 렌더링할 수 있다.
- 컴포넌트의 계층을 변경하지 않고도 DOM의 구조를 조작하고, DOM에 추가되는 부분을 제어하기 위해 사용되는 내장 컴포넌트이다.

  ```vue
  <!-- ManageGoals.vue -->
  <template>
    <div>
      <teleport to="body">
        <error-alert v-if="inputIsvalid">
          <h2>Input is invalid!</h2>
          <p>Please enter at least a few characters...</p>
          <button @click="confirmError">Okay</button>
        </error-alert>
      </teleport>
    </div>
  </template>
  ```

<br/>

**Vue 스타일 가이드**

- [Vue 스타일 가이드](https://vuejs.org/style-guide/)