<template>
  <div class="container" style ="margin-top:20px;">

    <div class="card row">
      <div class="card-body">
        <form id = "todo-form">
          <div class="form-row">
            <div class="form-group col-md-6">
              <input
                class="form-control"
                type="text"
                v-model="todoName"
                @keyup.enter="addTodo"
                placeholder="Enter a Todo">
            </div>
          </div>
          <button @click="addTodo" type="submit" class="btn btn-danger">Add a Todo</button>
        </form>
        <hr>

      </div>

      <div class="card-body">

        <h5 class="card-title">Todos</h5>
        <div class="form-row">
          <div class="form-group col-md-6">
            <input
              class="form-control"
              type="text"
              v-model="search"
              placeholder="Search for a Todo">
          </div>
        </div>

        <hr>
        <ul class="list-group">
          <li
            v-for="todo in filteredTodos" :key="todo.todo_id"
            class="list-group-item d-flex justify-content-between">
            {{ todo.description }}
            <a @click="deleteTodo(todo.todo_id)" class ="delete-item">
              <i class = "fa fa-remove"></i>
            </a>

          </li>
        </ul>
        <a id = "clear-todos" class="btn btn-dark">Clear All Todos</a>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'TodoInterface',

  mounted() {
    axios.get('http://localhost:5000/todos')
      .then((response) => {
        this.todos = response.data;
      })
      .catch((error) => {
        this.todos = console.log(error);
      });
  },
  data() {
    return {
      todos: [],
      todoName: '',
      search: '',
    };
  },
  methods: {
    addTodo() {
      axios.post('http://localhost:5000/todos', { description: this.todoName })
        .then((response) => {
          console.log(response);
        })
        .catch((e) => console.log(e));
    },
    deleteTodo(todoId) {
      axios.delete(`http://localhost:5000/todos/${todoId}`)
        .then((res) => {
          console.log(res);
        })
        .catch((err) => {
          console.error(err);
        });
    },
  },
  computed: {
    filteredTodos() {
      return this.todos.filter(({ description }) => description.match(this.search));
    },
  },
};
</script>

<style scoped>

</style>
