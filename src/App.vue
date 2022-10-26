<script setup>
import { computed, onMounted, ref, watch } from "vue";

const name = ref("");
const todos = ref([]);

const input_content = ref("");
const input_category = ref(null);

const todos_sorted = computed(() => {
  return todos.value.sort((a, b) => b.createdAt - a.createdAt);
});

const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null) {
    return;
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  });

  input_content.value = "";
  input_category.value = null;
};

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});

const removeTodo = (todo) =>
  (todos.value = todos.value.filter((t) => t != todo));
</script>

<template>
  <main class="app">
    <section class="header">
      <h2 class="main-heading">
        Hello Dear <input type="text" placeholder="your name" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <form @submit.prevent="addTodo" id="new-todo-form">
        <h4>Add your next todo!</h4>
        <input
          type="text"
          id="todo-content"
          placeholder="your next todo..."
          name="content"
          v-model="input_content"
        />

        <h4>Select your category.</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              id="category_first"
              value="work"
              v-model="input_category"
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
              v-model="input_category"
            />
            <span class="checkbox personal"></span>
            <p>Personal</p>
          </label>
        </div>
        <button>Add todo</button>
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST!</h3>
      <div class="list" id="todo-list">
        <div
          v-for="todo in todos_sorted"
          :key="todo.createdAt"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`checkbox ${todo.category}`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <div class="buttons">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>
