<template>
  <form @submit.prevent="handleSubmit" class="mb-4">
    <div class="flex gap-2">
      <input
        v-model="todoText"
        type="text"
        placeholder="Add a new todo..."
        class="flex-1 px-3 py-2 border rounded focus:outline-none focus:border-blue-500"
      />
      <button
        type="submit"
        class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
        :disabled="!todoText.trim()"
      >
        Add
      </button>
    </div>
  </form>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add-todo'])
const todoText = ref('')

const handleSubmit = () => {
  const text = todoText.value.trim()
  if (text) {
    emit('add-todo', text)
    todoText.value = ''
  }
}
</script>