<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')

const todos_asc = computed(() => todos.value.sort((a,b) => {
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
  if (input_content.value.trim() === '') {
    return
  }

  todos.value.push({
    content: input_content,
    done: false,
    createdAt: new Date().getItem()
  })
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
})

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
}, {deep: true})


onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos') || '[]')
})
</script>

<template>

<main class="app">
  <section class="create-todo">
    <h3>CREATE A TODO</h3>

    <form action="submit">
      <h4>What's on your todo list?</h4>
      <input 
        type="text" 
        placeholder="1"
        v-model="input_content">

        <input type="submit" value="Add todo">
    </form>
  </section>

  {{ todos }}
</main>

</template>