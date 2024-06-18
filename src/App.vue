<template>
  <v-app>

    <v-app-bar color="blue">
      <h1>Мой ToDo лист</h1>
    </v-app-bar>

  <v-container class = "app" >
    <h1>a</h1>

    <v-row class="input-section" >
      <v-col class="create-todo">
        <v-form @submit.prevent="addTodo">
          <h3>Какие планы на ближайшее время?</h3>
          <v-text-field placeholder="например: купить молоко" v-model="text"/>
          <v-btn variant="tonal" color="blue" type="submit">Добавить</v-btn>
        </v-form>
      </v-col>
    </v-row>

    <v-row class="todo-section" lines="one">
      <v-col class="todo-list">
        <h2 v-show="todos.length === 0">Нету дел на сегодня</h2>

        <v-list class="mx-auto"
        density="compact"
        max-width="500">
        <v-list-item 
        v-for="todo in todos" class="d-flex mb-2 bg-surface-variant d-row"
        :class="`todo-item ${todo.done && 'done'}`"
        :key="todo.id">
        
          <v-col class="d-flex ma-1 pa-1">
            <v-checkbox type="checkbox" :label="`${todo.todo}`"/>
          </v-col>

          <v-col class="d-flex ma-1 pa-1">
            <v-btn variant="tonal" color="red" @click="deleteTodo(todo)">Удалить</v-btn>
          </v-col>

        </v-list-item>
        </v-list>
    </v-col>
  </v-row>
  </v-container>
</v-app>
</template>

<script setup>
import {ref, onMounted, watch} from "vue";

const todos = ref([]);
const text = ref("");

function addTodo() {
  if(text.value.trim() === ""){return;}

  todos.value.unshift({
  todo: text.value,
  done: false,
});

text.value = "";
}

function deleteTodo(todo) {
  todos.value = todos.value.filter((x) => x !== todo);
}

watch(
  todos,
  (newTodoValue) => {
    localStorage.setItem("todos", JSON.stringify(newTodoValue));
  },
  {deep: true}
);

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>
