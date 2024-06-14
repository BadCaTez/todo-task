<template>
  <v-app>

    <v-app-bar color="blue">
      <h1>Мой ToDo лист</h1>
    </v-app-bar>

  <v-container class = "app" >

    <v-col class="gretting_text">
      <h3 class="title">Мой ToDo лист для WordPress</h3>
    </v-col>

    <v-row class="input-section" >
      <v-col class="create-todo">
        <v-form @submit.prevent="addTodo">
          <h3>Какие планы на ближайшее время?</h3>
          <v-text-field placeholder="например: купить молоко" v-model="text"/>
          <v-btn variant="tonal" color="blue" type="submit">Добавить</v-btn>
        </v-form>
      </v-col>
    </v-row>

    <v-list class="todo-section" lines="one">
      <v-col class="todo-list">
        <h2 v-show="todos.length === 0">Нету дел на сегодня</h2>

        <v-sheet class="d-flex flex-warp bg-surface-variant">
        <v-sheet v-for="todo in todos" class="ma-2 pa-2"
        :class="`todo-item ${todo.done && 'done'}`"
        :key="todo.id"
        >
        <v-label>
          <v-checkbox type="checkbox" v-model="todo.done"/>
        </v-label>

        <v-row class="todo-content">
          <v-text-field type="text" variant="plain" v-model="todo.todo"/>
        </v-row>
        <v-row class="actions">
          <v-button variant="tonal" color="red" @click="deleteTodo(todo)">Удалить</v-button>
        </v-row>
      </v-sheet>
    </v-sheet>
  </v-col>
</v-list>
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
