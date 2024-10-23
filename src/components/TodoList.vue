<!-- src/components/TodoList.vue -->
<template>
  <div class="todo-container">
    <h1>Karim's Simple To-Do List</h1>
    <form @submit.prevent="addTodo" class="todo-form">
      <input
          type="text"
          v-model="newTodo"
          placeholder="Add a new task"
          aria-label="Add a new task"
          required
          class="todo-input"
      />
      <button
          type="submit"
          :disabled="!newTodo.trim()"
          aria-label="Add Task Button"
          class="add-button">
        Add
      </button>
    </form>
    <ul class="todo-list">
      <TodoItem
          v-for="(todo, index) in todos"
          :key="index"
          :todo="todo"
          :index="index"
          @toggle-completion="toggleCompletion"
          @remove-todo="removeTodo"
      />
    </ul>
    <p v-if="todos.length === 0" class="no-tasks">No tasks added yet!</p>
  </div>
</template>

<script>
import TodoItem from '@/components/TodoItem.vue';

export default {
  name: 'TodoList',
  components: {
    TodoItem,
  },
  data() {
    return {
      newTodo: '',
      todos: [],
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push({ text: this.newTodo.trim(), completed: false });
        this.newTodo = '';
      }
    },
    toggleCompletion(payload) {
      const { index, completed } = payload;
      if (this.todos[index] !== undefined) {
        this.todos[index].completed = completed;
        console.log(`Todo "${this.todos[index].text}" marked as ${completed ? 'completed' : 'incomplete'}.`);
      } else {
        console.warn(`Todo at index ${index} is undefined.`);
      }
    },
    removeTodo(index) {
      if (this.todos[index] !== undefined) {
        console.log(`Removing todo: "${this.todos[index].text}"`);
        this.todos.splice(index, 1);
      } else {
        console.warn(`Cannot remove todo at index ${index} because it is undefined.`);
      }
    },
  },
};
</script>