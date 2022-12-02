<template>
  <div id="app">
    <h2>ToDoリスト</h2>
    <form @submit.prevent>
      <div class="l-flex">
        <div class="">名前</div>
        <input type="text" v-model="todoText" />
      </div>
      <TheButton @click="addItem()" text="追加" />
    </form>
    <ul>
      <li v-for="(todo, index) in todoList" :key="todo">
        <input type="checkbox" v-model="todo.isDone" />
        <span v-bind:class="{ done: todo.isDone }">{{ todo.item }}</span>
        <TheButton @onClick="editText(index)" text="編集" />
        <input type="text" v-if="todo.isOpen" v-model="todo.item" />
        <TheButton @onClick="deleteButton(index)" text="削除" />
      </li>
    </ul>
  </div>
</template>

<script setup>
  import { ref } from "vue";
  import TheButton from "../components/TheButton.vue";
  const todoList = ref([]);
  const todoText = ref("");
  const addItem = () => {
    if (todoText.value === '') return
    let todo = {
      item: todoText.value,
      isDone: false,
      isOpen: false
    };
    todoList.value.push(todo);
    todoText.value = "";
  };
  let deleteButton = (index) => {
    todoList.value.splice(index, 1);
  };
  let editText = (index) => {
    let editFrag =  todoList.value[index].isOpen
    if (editFrag) {
      todoList.value[index].isOpen = false
    } else {
      todoList.value[index].isOpen = true
    }
  }
</script>