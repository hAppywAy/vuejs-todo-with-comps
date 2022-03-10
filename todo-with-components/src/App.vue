<template>
  <div id="app" class="min-h-screen h-full" style="background: #edf2f7;">
    <div class="flex items-center justify-center">
      <div class="w-full flex items-center justify-center font-sans">
        <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg overflow-y-auto">
              <div class="mb-4">
                  <h1 class="text-3xl text-gray-900">Ma Todo</h1>
                  <todo-input @onAddTodo="description => addTodo(description)" />
              </div>
              <todo-sumup :percentageDone="percentageDone" />
              <div>
                  <todo-element v-for="todo in paginatedTodos" :key="todo.id" :todo="todo"
                    @onDeleteTodo="id => deleteTodo(id)"
                    @onToggleDone="id => toggleDone(id)"
                   />
              </div>
              <todo-pagination :todos="todos" @onSelectPage="page => selectPage(page)" :page="page" />
          </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoInput from "./components/TodoInput.vue";
import TodoSumup from "./components/TodoSumup.vue";
import TodoElement from "./components/TodoElement.vue";
import TodoPagination from "./components/TodoPagination.vue";

import config from "./config/page";

export default {
  name: 'App',
  components: {
    TodoInput,
    TodoSumup,
    TodoElement,
    TodoPagination
  },
  data() {
    return {
      description: "",
      page: 1,
      todos: [
        {
          id: 1,
          done: false,
          description: 'Ajouter une todo avec Vue',
        },
        {
          id: 2,
          done: true,
          description: 'Design de ma Todo',
        },
      ]
    }
  },
  computed: {
    paginatedTodos() {
      return this.todos.slice((this.page - 1) * config.PAGE_SIZE, (this.page - 1) * config.PAGE_SIZE + config.PAGE_SIZE);
    },
    percentageDone() {
      return `${this.todos.length > 0 ? (((this.todos.filter(todo => todo.done)).length / this.todos.length) * 100).toFixed(2) : 0}%`;
    },
    maxTodoId() {
      return Math.max(...this.todos.map(todo => todo.id));
    }
  },
  methods: {
    deleteTodo(id) {
      this.todos.splice(this.todos.findIndex(todo => todo.id === id), 1);
    },
    addTodo(description) {
      if (description) {
        this.todos.unshift({id: this.maxTodoId + 1, description, done: false});
      }
    },
    toggleDone(id) {
      const todoToEdit = this.todos.find(todo => todo.id === id);
      if (todoToEdit) {
        todoToEdit.done = !todoToEdit.done;
      }
    },
    selectPage(page) {
      this.page = page;
    }
  }
}
</script>
