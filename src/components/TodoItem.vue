<!-- src/components/TodoItem.vue -->
<template>
  <li class="todo-item">
    <input
        type="checkbox"
        :checked="todo.completed"
        @change="onToggleCompletion($event.target.checked)"
        aria-label="Mark task as completed"
        class="todo-checkbox"
    />
    <span :class="{ completed: todo.completed }" class="todo-text">{{ todo.text }}</span>
    <button @click="removeTodo" aria-label="Delete task" class="delete-button">✖</button>
  </li>
</template>

<script>
export default {
  name: 'TodoItem',
  props: {
    todo: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
  },
  methods: {
    onToggleCompletion(isCompleted) {
      this.$emit('toggle-completion', { index: this.index, completed: isCompleted });
      console.log(`Emitted toggle-completion for index ${this.index} with completed=${isCompleted}`);
    },
    removeTodo() {
      this.$emit('remove-todo', this.index);
      console.log(`Emitted remove-todo for index ${this.index}`);
    },
  },
};
</script>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  padding: 12px 16px;
  margin-bottom: 12px;
  background-color: #ffffff;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  transition: background-color 0.3s, box-shadow 0.3s;
}

.todo-item:hover {
  background-color: #f5f5f5;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
}

.todo-checkbox {
  margin-right: 16px;
  width: 20px;
  height: 20px;
  cursor: pointer;
  accent-color: #3498db; /* Modern blue color for the checkbox */
  transition: transform 0.2s;
}

.todo-checkbox:focus {
  outline: none;
  transform: scale(1.2);
}

.todo-text {
  flex: 1;
  font-size: 1rem;
  color: #2c3e50;
  transition: color 0.3s, text-decoration 0.3s;
}

.todo-text.completed {
  text-decoration: line-through;
  color: #95a5a6;
}

.delete-button {
  background: none;
  border: none;
  color: #e74c3c;
  font-size: 1.2rem;
  cursor: pointer;
  transition: color 0.3s, transform 0.2s;
}

.delete-button:hover {
  color: #c0392b;
  transform: scale(1.1);
}

.delete-button:focus {
  outline: none;
}

/* Responsive Design */
@media (max-width: 600px) {
  .todo-item {
    padding: 10px 14px;
  }

  .todo-text {
    font-size: 0.95rem;
  }

  .delete-button {
    font-size: 1rem;
  }
}
</style>
