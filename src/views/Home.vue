<template>
  <div class="home">
    <TodoInterface></TodoInterface>
  </div>
</template>

<script>
// @ is an alias to /src

import TodoInterface from '@/components/TodoInterface.vue';

export default {
  name: 'Home',
  components: {
    TodoInterface,
  },
};
</script>
