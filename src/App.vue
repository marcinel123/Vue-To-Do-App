<script setup>
import { computed, onMounted, ref, watch } from "vue";

const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_category = ref(null);

// const todos_sorted = computed();

watch(name, (newVal) => {
  localStorage.setItem("name", newVal)
})

onMounted(()=>{
  name.value = localStorage.getItem("name") || ""
})


</script>

<template>
  <main class="app">
    <section class="header">
      <h2 class="main-heading">
        Hello Dear <input 
        type="text" 
        placeholder="your name"
        v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <form id="new-todo-form">
        <h4>Add your next todo!</h4>
        <input
          type="text"
          id="todo-content"
          placeholder="your next todo..."
          name="content"
        />

        <h4>Select your category.</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              id="category_first"
              value="work"
            />
            <span class="checkbox work"></span>
            <p>Work</p>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              id="category_second"
              value="personal"
            />
            <span class="checkbox personal"></span>
            <p>Personal</p>
          </label>
        </div>
        <button type="submit">Add todo</button>
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST!</h3>
      <div class="list" id="todo-list"></div>
    </section>
  </main>
</template>
