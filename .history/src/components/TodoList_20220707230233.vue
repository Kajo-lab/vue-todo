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

	input_content.value = ''
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

<main class="bg-gradient-to-b from-gray-900 to-gray-700 bg-gradient-to-r">
	<div class="relative min-h-screen bg-white px-6 pt-10 pb-8 shadow-xl ring-1 ring-gray-900/5 sm:mx-auto sm:max-w-lg sm:px-10">
		<section class="mb-10">
		<h1 class="text-4xl text-yellow-500 mb-6">CREATE A TODO</h1>
				<h3 class="mb-2 font-bold">Add a todo</h3>
				<form id="new-todo-form" class="flex-none sm:flex" @submit.prevent="addTodo">
					<input
						class="break-words shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" 
						type="text" 
						name="content"
						id="content" 
						placeholder="e.g. Read a book"
						v-model="input_content" />

					<button type="submit" class="h-10 w-full sm:w-32 text-white rounded-lg bg-green-500 hover:bg-green-600 mt-4 sm:mt-0 ml-0 sm:ml-4">Add todo</button>
				</form>
		</section>

		<section>
				<h3 class="mb-2 font-bold">TODO LIST</h3>
				<div id="todo-list">

					<div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
						<div class="flex-none sm:flex todo-content items-center py-2">
							<div class="flex w-full shadow appearance-none border-none rounded py-2 px-3">
								<label>
									<input type="checkbox" v-model="todo.done" class="bg-gray-200 hover:bg-gray-300 cursor-pointer 
											w-6 h-6 border-3 border-amber-500 checked:bg-green-500 focus:outline-none rounded-full"/>
								</label>
									<input
									class="w-full appearance-none border-none rounded text-xl leading-tight focus:outline-none focus:shadow-outline" 
									type="text" 
									v-model="todo.content" />
							</div>
							<button class="h-10 w-full sm:w-20 text-white rounded-lg bg-red-500 hover:bg-red-600 my-4 sm:my-0 ml-0 sm:ml-4" @click="removeTodo(todo)">Delete</button>
						</div>
					</div>

				</div>
		</section>
	</div>
</main>

</template>

<style src="../assets/tailwind.css"></style>