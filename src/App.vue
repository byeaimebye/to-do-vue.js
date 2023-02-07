<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
  if(input_content.value.trim() === '' || input_category.value === null){
    return 
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })

  input_content.value = ''
  input_category.value = null
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {deep:true} )
//set value in local storage
watch(name, (newVal) => {
  localStorage.setItem("name", newVal)
})
//save the value
onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos') || [])
})

</script>

<template>
  <main class="app">

    <section class="greeting">
      <h2 class="title">
        What's up, <input className="input-name" type="text" class="text" placeholder="Insert user name" 
        v-model="name"/>
      </h2>
    </section>

    <section class="create-todo">
      <h3>Create a todo</h3>

      <form @submit.prevent="addTodo">
        <h4>What's on your todo</h4>
        <div class="input-todo">
        <input 
          type="text" 
          placeholder="e.g make a video"
          v-model="input_content"/>
        </div>
          <h4>Pick a category</h4>
          <div class="options">
            <div class="business">
            <label for="">
              <input 
                type="radio" 
                name="category" 
                value="business"
                v-model="input_category"/>
                <span class=""></span>
                <div>Business</div>
            </label>
          </div>
          <div class="personal">
            <label for="">
              <input 
                type="radio" 
                name="category" 
                value="personal"
                v-model="input_category"/>
                <span class=""></span>
                <div>Personal</div>
            </label>
          </div>
          </div>
      <input className="button-submit" type="submit" value="Add todo">

      </form>

    </section>

    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list">
        <div className="list-item" v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
        
          <label :className="`bubble ${todo.category}`">
            <input type="checkbox" v-model="todo.done">
            <span :className="`bubble ${todo.category}`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <div className="actions">
            <button class="delete" @click="removeTodo(todo)">
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M6 19C6 20.1 6.9 21 8 21H16C17.1 21 18 20.1 18 19V7H6V19ZM9.17 12.59C8.98302 12.403 8.87798 12.1494 8.87798 11.885C8.87798 11.6206 8.98302 11.367 9.17 11.18C9.35698 10.993 9.61057 10.888 9.875 10.888C10.1394 10.888 10.393 10.993 10.58 11.18L12 12.59L13.41 11.18C13.597 10.993 13.8506 10.888 14.115 10.888C14.3794 10.888 14.633 10.993 14.82 11.18C15.007 11.367 15.112 11.6206 15.112 11.885C15.112 12.1494 15.007 12.403 14.82 12.59L13.41 14L14.82 15.41C14.9126 15.5026 14.986 15.6125 15.0361 15.7335C15.0862 15.8544 15.112 15.9841 15.112 16.115C15.112 16.2459 15.0862 16.3756 15.0361 16.4965C14.986 16.6175 14.9126 16.7274 14.82 16.82C14.7274 16.9126 14.6175 16.986 14.4965 17.0361C14.3756 17.0862 14.2459 17.112 14.115 17.112C13.9841 17.112 13.8544 17.0862 13.7335 17.0361C13.6125 16.986 13.5026 16.9126 13.41 16.82L12 15.41L10.59 16.82C10.4974 16.9126 10.3875 16.986 10.2665 17.0361C10.1456 17.0862 10.0159 17.112 9.885 17.112C9.75407 17.112 9.62442 17.0862 9.50346 17.0361C9.38249 16.986 9.27258 16.9126 9.18 16.82C9.08742 16.7274 9.01398 16.6175 8.96387 16.4965C8.91377 16.3756 8.88798 16.2459 8.88798 16.115C8.88798 15.9841 8.91377 15.8544 8.96387 15.7335C9.01398 15.6125 9.08742 15.5026 9.18 15.41L10.59 14L9.17 12.59ZM18 4H15.5L14.79 3.29C14.61 3.11 14.35 3 14.09 3H9.91C9.65 3 9.39 3.11 9.21 3.29L8.5 4H6C5.45 4 5 4.45 5 5C5 5.55 5.45 6 6 6H18C18.55 6 19 5.55 19 5C19 4.45 18.55 4 18 4Z" fill="black"/>
              </svg>
            </button>
          </div>
          
        </div>
      </div>
    </section>
  </main>
</template>
