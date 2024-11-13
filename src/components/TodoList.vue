<template>
  <div class="space-y-2">
    <div 
      v-for="todo in todos" 
      :key="todo.id"
      class="flex items-center gap-2 p-3 bg-white rounded shadow"
    >
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="$emit('toggle-todo', todo.id)"
        class="w-5 h-5"
      />
      <span 
        :class="['flex-1', todo.completed ? 'line-through text-gray-500' : '']"
      >
        {{ todo.text }}
      </span>
      <button
        @click="$emit('delete-todo', todo.id)"
        class="px-2 py-1 text-red-500 hover:bg-red-100 rounded"
      >
        Delete
      </button>
    </div>
    
    <div v-if="todos.length === 0" class="text-center text-gray-500 py-4">
      No todos yet. Add one above!
    </div>
  </div>
</template>

<script setup>
defineProps({
  todos: {
    type: Array,
    required: true
  }
})

defineEmits(['toggle-todo', 'delete-todo'])
</script>