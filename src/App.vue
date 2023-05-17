<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((first, second) => {
  return second.createdAt - first.createdAt
}))

const addTodo = () => {
  const has_content = input_content.value.trim() === ''
  const is_null = input_content.value === null

  if (has_content || is_null) {
    return
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })
}

watch(todos, newValue => {
  localStorage.setItem('todos', JSON.stringify(newValue))
}, { deep: true })

watch(name, (newValue) => { localStorage.setItem('name', newValue) })

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos') || [])
})

</script>


<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        what's up, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3 class="create-todo"> CREATE A TODO</h3>

      <form @submit.prevent="addTodo">
        <h4> what's on your todo list?</h4>
        <input type="text" placeholder="e.g. make a video" v-model="input_content">

        <h4>Pick a category</h4>

        <div class="options">
          <label>
            <input type="radio" name="category" value="business" v-model="input_category">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
        </div>

        <div class="options">
          <label>
            <input type="radio" name="category" value="personal" v-model="input_category">
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo">
      </form>
    </section>

    {{ todos_asc }}
  </main>
</template>
