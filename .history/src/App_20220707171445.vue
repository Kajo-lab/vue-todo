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

<main class="relative flex min-h-screen flex-col justify-center overflow-hidden bg-gray-200 to-black py-6 sm:py-12">
	<div class="relative bg-white px-6 pt-10 pb-8 shadow-xl ring-1 ring-gray-900/5 sm:mx-auto sm:max-w-lg sm:rounded-lg sm:px-10">
		<section class="mb-10">
		<h1 class="text-4xl text-yellow-500 mb-6">CREATE A TODO</h1>
				<h3 class="mb-2 font-bold">Add a todo</h3>
				<form id="new-todo-form" class="flex gap-6" @submit.prevent="addTodo">
					<input
						class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" 
						type="text" 
						name="content" 
						id="content" 
						placeholder="e.g. "
						v-model="input_content" />

					<input type="submit" value="Add todo" />
				</form>
		</section>

		<section>
				<h3 class="mb-2 font-bold">TODO LIST</h3>
				<div id="todo-list ">
					<div class="flex mb-4" v-for="todo in todos_asc" :key="`todo-item ${todo.done && 'done'}`">

						<div class="flex gap-6 ">
							<input
							class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" 
							type="text" 
							v-model="todo.content" />

							<button class="h-10 w-20 text-white rounded-lg bg-red-500 hover:bg-red-600" @click="removeTodo(todo)">Delete</button>
						</div>
					</div>
				</div>
		</section>
	</div>
</main>

</template>

<style src="./assets/tailwind.css"></style>