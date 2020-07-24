<template>
  <div>
    <ul>
      <TodoItem
        v-for="(todo, i) of todos"
        :todo="todo"
        :index="i"
        @remove-todo="removeTodo"
        @toggle-status-todo="toggleStatusTodo"
      />
    </ul>
  </div>
</template>

<script>
  import TodoItem from './TodoItem';
  export default {
    props: {
      todos: {
        type: Array,
      }
    },
    components: { TodoItem },
    methods: {
      removeTodo(id) {
        this.$emit('remove-todo', id);
      },
      toggleStatusTodo(id) {
        this.$emit('toggle-status-todo', id);
      }
    },
  }
</script>

<style scoped>
  ul {
    max-width: 300px;
    padding: 0 15px;
    list-style: none;
    margin: 0 auto;
  }
</style>
