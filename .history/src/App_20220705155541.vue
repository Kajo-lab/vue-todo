<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b ) => {
  return b.createdAt - a.createdAt
}))

</script>

<template>

<main class="app">
  <div class="section greeting">
    <h2 class="title">
      What's up, <input type="text" placeholder="name here"
      v-model="name" />
    </h2>
  </div>
</main>

</template>