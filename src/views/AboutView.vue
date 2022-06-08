<template>
  <div class="about">
    <h1>This is an about page</h1>
    <div class="area">
      <h1 class="title">
        タイトル
      </h1>
      <div>
        ref を使ったv-model
        <input type="text" v-model="data">
        refの入力されたデータ⇨{{ data }}
      </div>
      <div>
        reactive を使ったv-model
        <input type="text" v-model="form.text">
        reactiveの入力されたデータ⇨{{ form.text }}
      </div>
      <TheButton @onClick="buttonAction" text="ボタン"/>

      <br>
      <h2>Todo</h2>
      <form v-on:submit.prevent>
        <input type="text" v-model="newItem">
        <TheButton type="button" text="追加" @onClick="addItem"/>
      </form>
      <ul>
        <li v-for="(todo, index) in todos" :key="todo">
          <input type="checkbox" v-model="todo.isDone"/>
          <span v-bind:class="{ done: todo.isDone }">{{ todo.item }}</span>
          <TheButton @onClick="deleteButton(index)" text="削除"/>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref, reactive } from 'vue';
import TheButton from '../components/TheButton.vue'

export default {
  props: {},
  components: {
    TheButton
  },
  setup() {
    // ref のデータ 「data」でアクセスできる
    const data = ref('refデータ')
    // reactive のデータ 「form.text」でアクセスできる
    const form = reactive({
      text: "reactiveデータ",
    });

    const buttonAction = () => {
      alert('ボタンが押されました')
    }


    // Todoリスト
    let todos = ref([])
    let newItem = ref('')
    let addItem = () => {
      if (newItem.value === '') return
      let todo = {
        item: newItem.value,
        isDone: false
      }
      todos.value.push(todo)
      newItem.value = ""
    }
    let deleteButton = (index) => {
      todos.value.splice(index , 1)
    }

    return {
      form,
      data,
      buttonAction,
      addItem,
      newItem,
      todos,
      deleteButton
    };
  },
}
</script>