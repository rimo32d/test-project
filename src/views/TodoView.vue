<template>
  <div id="app">
    <h2>ToDoリスト</h2>
    <form @submit.prevent>
      <div class="l-flex">
        <div class="">
          名前
        </div>
        <input type="text" v-model="todoText"/>
      </div>
      <button class="" type="button" @click="addItem()">追加</button>
    </form>
    <div class="">{{ todoList }}</div>
  </div>
</template>

<script setup>
  import { ref } from 'vue';
  const todoList = ref([])
  const todoText = ref('')
  const addItem = () => {
    todoList.value.push(todoText.value)
    todoText.value = ''
  }
</script>