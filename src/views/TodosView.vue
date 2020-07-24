<template>
  <div>
    <router-link to="/">Home</router-link>
    <h1>Todo App</h1>
    <hr>
    <AddTodo
      @add-todo="addTodo"
    />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="done">Done</option>
      <option value="processing">Processing</option>
    </select>
    <Loader v-if="isLoading" />
    <TodoList
      v-else-if="filteredTodos.length > 0"
      :todos="filteredTodos"
      @remove-todo="removeTodo"
      @toggle-status-todo="toggleStatusTodo"
    />
    <p v-else>All tasks completed!</p>
  </div>
</template>

<script>


  import AddTodo from '../components/AddTodo';
  import TodoList from '../components/TodoList';
  import Loader from '../components/Loader';

  export default {
    name: 'App',
    data() {
      return {
        todos: [],
        isLoading: true,
        filter: 'all',
      }
    },
    mounted() {
      const localCache = JSON.parse(localStorage.getItem('todoList'));
      const localfilter = JSON.parse(localStorage.getItem('filter'));
      if (localCache.length > 0) {
        this.todos = localCache;
        this.isLoading = false;
      } else {
        fetch('https://jsonplaceholder.typicode.com/todos/1')
          .then(response => response.json())
          .then(json => this.todos.push({ id: Date.now(), title: json.title, isDone: json.completed }))
          .then(() => this.isLoading = false);
      }
      this.filter = localfilter ? localfilter : 'all';
    },
    components: { Loader, AddTodo, TodoList },
    methods: {
      removeTodo(id) {
        this.todos = this.todos.filter(todo => todo.id !== id);
      },
      toggleStatusTodo(id) {
        this.todos = this.todos.map(todo => {
          if (todo.id === id) {
            return { ...todo, isDone: !todo.isDone }
          } else return todo;
        })
      },
      addTodo(todo) {
        this.todos.push(todo);
      },
    },
    watch: {
      todos: {
        handler: function () {
          localStorage.setItem('todoList', JSON.stringify(this.todos));
        },
      },
      // filter(value) {
      //   console.log(value);
      // }
    },
    computed: {
      filteredTodos() {
        localStorage.setItem('filter', JSON.stringify(this.filter));
        if (this.filter === 'all') return this.todos;
        else if (this.filter === 'done') return this.todos.filter(todo => todo.isDone);
        else if (this.filter === 'processing') return this.todos.filter(todo => !todo.isDone);
      }
    }
  }
</script>
