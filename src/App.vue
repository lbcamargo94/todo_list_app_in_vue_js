<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) =>
{
  return a.createdAt - b.createdAt
}))

const addTodo = () => { }

watch(name, (newValue) => { localStorage.setItem('name', newValue) })
onMounted(() => { localStorage.getItem('name') })
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
          <label for="">
            <input type="radio" name="category" value="business" v-model="input_category">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
        </div>

        <div class="options">
          <label for="">
            <input type="radio" name="category" value="personal" v-model="input_category">
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo">
      </form>
    </section>
  </main>
</template>
