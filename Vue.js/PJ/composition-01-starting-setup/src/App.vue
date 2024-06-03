<template>
  <section class="container">
    <h2>{{ user.name }}</h2>
    <h3>{{ user.age }}</h3>
  </section>
</template>

<script>
import { reactive } from 'vue';

export default {
  setup() {
    // const uName = ref('Kangduoh');
    // const uAge = ref(26);
    const user = reactive({
      name: 'Kangduoh',
      age: 26
    });

    setTimeout(function() {
      // uName.value = 'duoh;
      // uAge.value = 27;
      user.name = 'duoh';
      user.age = 27;

    }, 2000);

    return { user: user };
  }
  // data() {
  //   return {
  //     userName: 'Maximilian',
  //   };
  // },
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}

.container {
  margin: 3rem auto;
  max-width: 30rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  text-align: center;
}
</style>