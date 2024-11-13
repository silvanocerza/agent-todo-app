<template>
  <div class="container mx-auto p-4 max-w-2xl">
    <h1 class="text-3xl font-bold mb-6 text-center">Todo App</h1>
    
    <!-- Todo input component -->
    <TodoInput @add-todo="addTodo" />
    
    <!-- Filter buttons -->
    <div class="flex gap-2 mb-4">
      <button 
        v-for="filter in filters" 
        :key="filter"
        @click="currentFilter = filter"
        :class="['px-3 py-1 rounded', 
                currentFilter === filter ? 'bg-blue-500 text-white' : 'bg-gray-200']"
      >
        {{ filter }}
      </button>
    </div>
    
    <!-- Todo list component -->
    <TodoList 
      :todos="filteredTodos"
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import TodoList from './components/TodoList.vue'
import TodoInput from './components/TodoInput.vue'

// Todo items state
const todos = ref([])

// Filtering
const filters = ['All', 'Active', 'Completed']
const currentFilter = ref('All')

// Computed filtered todos
const filteredTodos = computed(() => {
  switch (currentFilter.value) {
    case 'Active':
      return todos.value.filter(todo => !todo.completed)
    case 'Completed':
      return todos.value.filter(todo => todo.completed)
    default:
      return todos.value
  }
})

// Add new todo
const addTodo = (text) => {
  todos.value.push({
    id: Date.now(),
    text,
    completed: false
  })
  saveTodos()
}

// Toggle todo completion
const toggleTodo = (id) => {
  const todo = todos.value.find(todo => todo.id === id)
  if (todo) {
    todo.completed = !todo.completed
    saveTodos()
  }
}

// Delete todo
const deleteTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
  saveTodos()
}

// Local storage functions
const saveTodos = () => {
  localStorage.setItem('todos', JSON.stringify(todos.value))
}

// Load todos from localStorage on mount
const loadTodos = () => {
  const saved = localStorage.getItem('todos')
  if (saved) {
    todos.value = JSON.parse(saved)
  }
}

loadTodos()
</script>