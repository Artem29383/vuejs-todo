<template>
  <form @submit.prevent="addTodo">
    <input
      v-model="value"
      type="text">
  </form>
</template>

<script>
  export default {
    name: 'AddTodo',
    data() {
      return {
        value: '',
      }
    },
    methods: {
      addTodo() {
        if (this.value.trim()) {
          const todo = {
            id: Date.now(),
            title: this.value,
            isDone: false
          };
          this.$emit('add-todo', todo);
          this.value = '';
        }
      }
    }
  }
</script>

<style scoped>
  form {
    max-width: 300px;
    padding: 15px 15px 15px 30px;
    margin: 0 auto;
    justify-content: center;
    width: 100%;
    display: flex;
  }
</style>
