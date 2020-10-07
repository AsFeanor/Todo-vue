<template>
  <div class="container" style ="margin-top:20px;">

    <div class="card row">
      <div class="card-body">
        <form id = "todo-form" name="form">
          <div class="form-row">
            <div class="form-group col-md-6">
              <input
                class="form-control"
                type="text"
                name="todo"
                id = "todo"
                placeholder="Enter a Todo">
            </div>
          </div>
          <button type="submit" class="btn btn-danger">Add a Todo</button>
        </form>
        <hr>

      </div>

      <div class="card-body">

        <h5 class="card-title" id = "tasks-title">Todos</h5>
        <div class="form-row">
          <div class="form-group col-md-6">
            <input
              class="form-control"
              type="text"
              name="filter"
              id = "filter"
              placeholder="Search for a Todo">
          </div>
        </div>

        <hr>
        <ul class="list-group">
          <Todo></Todo>
        </ul>

        <a id = "clear-todos" class="btn btn-dark" href="#">Clear All Todos</a>
      </div>
    </div>

  </div>
</template>

<script>
import Todo from '@/components/Todo.vue';

export default {
  name: 'TodoInterface',
  components: Todo,
};
</script>

<style scoped>

</style>
