<script setup>
import { ref, computed } from 'vue'

const todos = ref([])
const newTodo = ref('')
let nextId = 1

const today = computed(() => {
  return new Date().toLocaleDateString('en-US', {
    month: 'short',
    day: 'numeric',
  })
})

function addTodo() {
  const text = newTodo.value.trim()
  if (!text) return
  todos.value.push({ id: nextId++, text, done: false })
  newTodo.value = ''
}

function removeTodo(id) {
  todos.value = todos.value.filter(t => t.id !== id)
}
</script>

<template>
  <div class="card">
    <header class="card-header">
      <h1>My Todo List</h1>
      <span class="date">{{ today }}</span>
    </header>

    <ul v-if="todos.length" class="todo-list">
      <li v-for="todo in todos" :key="todo.id" class="todo-item">
        <label class="todo-label">
          <input type="checkbox" v-model="todo.done" />
          <span class="checkmark"></span>
          <span class="todo-text" :class="{ done: todo.done }">{{ todo.text }}</span>
        </label>
        <button class="delete-btn" @click="removeTodo(todo.id)" aria-label="Delete">
          <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
            <path d="M1 1l12 12M13 1L1 13" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
          </svg>
        </button>
      </li>
    </ul>

    <p v-else class="empty">No tasks yet. Add one below!</p>

    <div class="add-row">
      <input
        v-model="newTodo"
        @keydown.enter="addTodo"
        placeholder="Add a new task..."
        class="add-input"
        maxlength="200"
        aria-label="New task"
      />
      <button class="add-btn" @click="addTodo" aria-label="Add task">+</button>
    </div>
  </div>
</template>

<style scoped>
.card {
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.08);
  overflow: hidden;
}

.card-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.5rem 1.5rem 1rem;
  border-bottom: 1px solid #f0f0f0;
}

.card-header h1 {
  font-size: 1.25rem;
  font-weight: 700;
  color: #1a1a1a;
}

.date {
  font-size: 0.875rem;
  color: #888;
}

.todo-list {
  list-style: none;
  padding: 0.5rem 0;
}

.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.625rem 1.5rem;
  gap: 0.5rem;
  transition: background 0.15s;
}

.todo-item:hover {
  background: #fafafa;
}

.todo-label {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex: 1;
  cursor: pointer;
  min-width: 0;
}

.todo-label input[type="checkbox"] {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

.checkmark {
  flex-shrink: 0;
  width: 18px;
  height: 18px;
  border: 2px solid #d1d5db;
  border-radius: 5px;
  transition: all 0.15s;
  position: relative;
}

.todo-label input[type="checkbox"]:checked ~ .checkmark {
  background: #6366f1;
  border-color: #6366f1;
}

.todo-label input[type="checkbox"]:checked ~ .checkmark::after {
  content: '';
  position: absolute;
  left: 4px;
  top: 1px;
  width: 5px;
  height: 9px;
  border: 2px solid #fff;
  border-top: none;
  border-left: none;
  transform: rotate(45deg);
}

.todo-text {
  font-size: 0.9375rem;
  color: #1a1a1a;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  transition: all 0.2s;
}

.todo-text.done {
  text-decoration: line-through;
  color: #aaa;
}

.delete-btn {
  flex-shrink: 0;
  width: 28px;
  height: 28px;
  padding: 0;
  border: none;
  background: transparent;
  color: #ccc;
  border-radius: 6px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.15s;
  opacity: 0;
}

.todo-item:hover .delete-btn {
  opacity: 1;
}

.delete-btn:hover {
  background: #fee2e2;
  color: #ef4444;
}

.delete-btn:focus-visible {
  opacity: 1;
  outline: 2px solid #6366f1;
  outline-offset: 2px;
}

.empty {
  text-align: center;
  padding: 2rem 1.5rem;
  color: #aaa;
  font-size: 0.9375rem;
}

.add-row {
  display: flex;
  gap: 0.5rem;
  padding: 1rem 1.5rem 1.5rem;
  border-top: 1px solid #f0f0f0;
}

.add-input {
  flex: 1;
  padding: 0.625rem 0.875rem;
  border: 1.5px solid #e5e7eb;
  border-radius: 8px;
  font-size: 0.9375rem;
  font-family: inherit;
  color: #1a1a1a;
  outline: none;
  transition: border-color 0.15s;
}

.add-input::placeholder {
  color: #bbb;
}

.add-input:focus-visible {
  border-color: #6366f1;
  outline: 2px solid #6366f1;
  outline-offset: 0px;
}

.add-btn {
  width: 40px;
  height: 40px;
  padding: 0;
  border: none;
  background: #6366f1;
  color: #fff;
  font-size: 1.25rem;
  font-weight: 400;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  line-height: 1;
  transition: background 0.15s;
}

.add-btn:hover {
  background: #4f46e5;
}
</style>
