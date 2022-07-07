<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')

const todos_asc = computed(() => todos.value.sort((a,b) =>{
	return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
	localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, {
	deep: true
})

const addTodo = () => {
  if (input_content.value.trim() === '') {
    return
  }

	todos.value.push({
		content: input_content.value,
		done: false,
		editable: false,
		createdAt: new Date().getTime()
	})
}

const removeTodo = (todo) => {
	todos.value = todos.value.filter((t) => t !== todo)
}

onMounted(() => {
	name.value = localStorage.getItem('name') || ''
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>

<main class="relative flex min-h-screen flex-col justify-center overflow-hidden bg-gray-50 py-6 sm:py-12">
	<div class="relative bg-white px-6 pt-10 pb-8 shadow-xl ring-1 ring-gray-900/5 sm:mx-auto sm:max-w-lg sm:rounded-lg sm:px-10">
		<section class="create-todo">
		<h1 class="text-">CREATE A TODO</h1>
				<form id="new-todo-form" class="" @submit.prevent="addTodo">
					<h4>What's on your todo list?</h4>
					<input
						class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" 
						type="text" 
						name="content" 
						id="content" 
						placeholder="e.g. make a video"
						v-model="input_content" />

					<input type="submit" value="Add todo" />
				</form>
		</section>

		<section class="todo-list">
				<h3>TODO LIST</h3>

				<div class="list" id="todo-list">
					<div v-for="todo in todos_asc" :key="`todo-item ${todo.done && 'done'}`">

						<div class="todo-content">
							<input type="text" v-model="todo.content" />
						</div>

						<div class="actions">
							<button class="delete" @click="removeTodo(todo)">Delete</button>
						</div>
					</div>
				</div>
		</section>
	</div>
</main>

</template>

<style src="./assets/tailwind.css"></style>