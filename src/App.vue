<template>
  <Layout>
    <template #header>
      Header
    </template>
  </Layout>
  <Layout>
    <template #aside>
      Sidebar
    </template>
  </Layout>
  <Layout>
    <template #main>
      Main
    </template>
  </Layout>
  <Layout>
    <template #footer>
      Footer
    </template>
  </Layout>
  <Button>
    <strong>Demo</strong> de bouton
  </Button>
  <main class="scroll-container">
    <!-- Main title -->
    <h1>TodoList-Vue</h1>

    <!-- Form -->

    <form action="" @submit.prevent="addTodo">
      <fieldset role="group">
        <input v-model="newTodo" type="text" placeholder="Nouvelle tâche">
        <button :disabled="newTodo.length == 0">Ajouter</button>
      </fieldset>
    </form>

    <!-- Hide completed tasked  -->
    <label>
      <input type="checkbox" v-model="hideCompleted">
      Masquer les tâches complétées
    </label>
    <p v-if="remainingTodos > 0">Il reste {{ remainingTodos }} tâche{{ remainingTodos > 1 ? 's' : '' }} à compléter</p>
    <!-- <Checkbox label="Bonjour" /> -->
    <hr>

    <!-- Generated Todo List -->
    <div v-if="todos.length === 0">Aucune tâche à effectuer</div>
    <div v-else class="todolist">
      <ul>
        <li v-for="todo in sortedTodos" :key="todo.date" :class="{ completed: todo.completed }">
          <!-- <label>
            <input type="checkbox" v-model="todo.completed">
            {{ todo.title }}
          </label>
          <Checkbox :label="todo.title" @check="(p) => console.log('coché', p)" @uncheck="console.log('déchoché')" /> -->
          <Checkbox :label="todo.title" v-model="todo.completed" />
        </li>
      </ul>
    </div>
  </main>
  <!-- <css-doodle>
    :doodle {
    @grid: 7 / 100vmax / #0a0c27;
    position: absolute;
    top: 0; left: 0;
    z-index: -1;
    }
    @shape: clover 5;
    background: hsla(-@i(*4), 70%, 68%, @r.8);
    transform:
    scale(@r(.2, 1.5))
    translate(@m2.@r(±50%));
  </css-doodle> -->
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'
import Checkbox from './Checkbox.vue'
import Button from './Button.vue'
import Layout from './Layout.vue'


const newTodo = ref('');
const hideCompleted = ref(false);
const todos = ref([]);

onMounted(() => {
  fetch('https://jsonplaceholder.typicode.com/todos')
    .then(r => r.json())
    .then(v => todos.value = v.map(todo => ({ ...todo, date: todo.id })))
})

const addTodo = () => {
  todos.value.push({
    title: newTodo.value,
    completed: false,
    date: Date.now()
  })
  newTodo.value = ''
};

const sortedTodos = computed(() => {
  console.log('demo')
  const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
  if (hideCompleted.value === true) {
    return sortedTodos.filter(t => t.completed === false)
  }
  return sortedTodos
});

const remainingTodos = computed(() => {
  return todos.value.filter(t => t.completed === false).length
});

</script>

<style>
li {
  list-style-type: none;
}

[v-cloak] {
  display: none;
}

.completed {
  opacity: 0.5;
  text-decoration: line-through;
}
</style>

