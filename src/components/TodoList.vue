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

<style scoped>
.todo-container {
  max-width: 500px;
  margin: 60px auto;
  padding: 30px;
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 25px;
  font-size: 2.5rem;
  color: #34495e;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.todo-form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 12px 15px;
  border: 2px solid #ecf0f1;
  border-radius: 8px;
  font-size: 1rem;
  transition: border-color 0.3s;
}

.todo-input:focus {
  border-color: #3498db;
  outline: none;
}

.add-button {
  padding: 12px 20px;
  background-color: #3498db;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

.add-button:disabled {
  background-color: #bdc3c7;
  cursor: not-allowed;
}

.add-button:not(:disabled):hover {
  background-color: #2980b9;
}

.add-button:not(:disabled):active {
  transform: scale(0.98);
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.no-tasks {
  text-align: center;
  color: #7f8c8d;
  font-size: 1.1rem;
  margin-top: 15px;
}

/* Responsive Design */
@media (max-width: 600px) {
  .todo-container {
    margin: 30px 20px;
    padding: 20px;
  }

  h1 {
    font-size: 2rem;
  }

  .todo-input, .add-button {
    font-size: 0.9rem;
    padding: 10px 12px;
  }
}
</style>
