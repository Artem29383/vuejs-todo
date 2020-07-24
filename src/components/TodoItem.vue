<template>
  <li>
    <span :class="{done: todo.isDone}">
       <input type="checkbox" :checked="todo.isDone" @change="toggleStatusTodo">
       <strong class="number">{{index + 1}}</strong>
      {{todo.title | upperCase }}
  </span>
    <button
      class="rm"
      @click="$emit('remove-todo', todo.id)"
    >
      &times;
    </button>
  </li>
</template>

<script>
  export default {
    props: {
      todo: {
        type: Object,
        required: true,
      },
      index: {
        type: Number,
      }
    },
    methods: {
      toggleStatusTodo() {
        this.$emit('toggle-status-todo', this.todo.id);
      }
    },
    filters: {
      upperCase(value) {
        return value.toUpperCase();
      }
    },
  }
</script>

<style scoped>
  li {
    display: flex;
    justify-content: space-between;
    width: 100%;
    padding: 5px;
    border: 1px solid black;
    margin-bottom: 5px;
  }

  .number {
    margin: 0 auto 0 5px;
  }

  .rm {
    border-radius: 50%;
    background-color: red;
    color: #fff;
    border: none;
    font-weight: bold;
  }

  .done {
    text-decoration: line-through;
  }
</style>
