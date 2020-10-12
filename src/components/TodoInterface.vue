<template>
  <div class="container" style ="margin-top:20px;">

    <div class="card row">
      <div class="card-body">
        <form @submit.prevent="submit">
          <div class="form-row">
            <div class="form-group col-md-6" :class="{ 'form-group--error': $v.todoName.$error }">
              <input
                class="form-control"
                type="text"
                v-model.trim="$v.todoName.$model"
                placeholder="Enter a Todo">
            </div>
            <div class="error"
                 v-if="!$v.todoName.required">Todo is required</div>
            <div class="error"
                 v-if="!$v.todoName.minLength">
              Todo must have at least {{$v.todoName.$params.minLength.min}} letters.
            </div>
          </div>
          <button @click="submit"
                  type="submit"
                  :disabled="submitStatus === 'PENDING'"
                  class="btn btn-danger">Add a Todo</button>
          <p class="typo__p" v-if="submitStatus === 'OK'">Thanks for your submission!</p>
          <p class="typo__p" v-if="submitStatus === 'ERROR'">Please fill the blank correctly.</p>
          <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
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
        <ul class="list-group" id="todoList">
          <li
            v-for="todo in filteredTodos" :key="todo.todo_id"
            class="list-group-item d-flex justify-content-between">
            {{ todo.description }}
            <a @click="deleteTodo(todo.todo_id)" class ="delete-item" :href="'/'">
              <i class = "fa fa-remove"></i>
            </a>
          </li>
        </ul>
        <a @click="clearTodos(todos)"
           :href="'/'"
           class="btn btn-dark">
          Clear All Todos</a>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import { required, minLength } from 'vuelidate/lib/validators';

export default {
  name: 'TodoInterface',

  mounted() {
    axios.get('http://localhost:5000/todos')
      .then((response) => {
        this.todos = response.data;
      })
      .catch((e) => {
        this.todos = console.log(e);
      });
  },
  data() {
    return {
      todos: [],
      todoName: '',
      search: '',
      submitStatus: null,
    };
  },
  validations: {
    todoName: {
      required,
      minLength: minLength(4),
    },
  },
  methods: {
    deleteTodo(todoId) {
      axios.delete(`http://localhost:5000/todos/${todoId}`)
        .then((res) => {
          console.log(res);
        })
        .catch((err) => {
          console.error(err);
        });
    },
    submit() {
      console.log('submit!');
      this.$v.$touch();
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR';
      } else {
        axios.post('http://localhost:5000/todos', { description: this.todoName })
          .then((response) => {
            console.log(response);
          })
          .catch((e) => console.log(e));
        this.submitStatus = 'PENDING';
        setTimeout(() => {
          this.submitStatus = 'OK';
        }, 500);
      }
    },
    clearTodos() {
      axios.delete('http://localhost:5000/todos')
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
