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
        <span v-bind:class="{ done: todo.isDone }">
          {{ todo.item }}
        </span>
        <TheButton @onClick="editText(index)" text="編集" />

        <input type="text" v-if="todo.isOpen" v-model="todo.item" />
        <TheButton @onClick="deleteButton(index)" text="削除" />
      </li>
    </ul>
  </div>
  <div id="trello" class="l-flex" style="align-items: flex-start;">
    <div
      v-for="(list, index) in lists"
      :key="index"
      style="min-width: 260px"
      class="c-list"
    >
      <div class="c-list--title">{{ list.name }}</div>
      <!-- 分類表示 -->
      <div>
        <div
          v-for="(card, index) in list.cards"
          :key="index"
          class="c-card"
          @mousedown="mousedown()"
        >
          <div class="c-card--inner">
            <div class="c-card--title">
              {{ card.name }}
            </div>
            <div class="c-card--desc" v-show="card.description">
              {{ card.description }}
            </div>
            <div class="c-card--user">
              {{ card.user_name }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref,onMounted } from "vue";
import TheButton from "../components/TheButton.vue";
const todoList = ref([]);
const todoText = ref("");
const addItem = () => {
  if (todoText.value === "") return;
  let todo = {
    item: todoText.value,
    isDone: false,
    isOpen: false,
  };
  todoList.value.push(todo);
  todoText.value = "";
};
let deleteButton = (index) => {
  todoList.value.splice(index, 1);
};
let editText = (index) => {
  let editFrag = todoList.value[index].isOpen;
  if (editFrag) {
    todoList.value[index].isOpen = false;
  } else {
    todoList.value[index].isOpen = true;
  }
};
let lists = ref([
  {
    id: 1,
    name: "ToDo",
    cards: [
      {
        id: 1,
        name: "レポートの作成",
        description: "コロナに影響による飲食店の倒産件数の調査",
        user_name: "鈴木",
      }
    ],
  },
  {
    id: 2,
    name: "作業中",
    cards: [
      {
        id: 4,
        name: "見積もりの作成",
        description: "",
        user_name: "山田",
      },
    ],
  },
  {
    id: 3,
    name: "完了",
    cards: [
      {
        id: 5,
        name: "B社への支払い",
        description: "経理への連絡を忘れないように",
        user_name: "鈴木",
      },
    ],
  },
]);
let mousedown = () => {
  console.log('click mousedown')
}
let mouseMove = () => {
  console.log('click mousemove')
}
let mouseUp = () => {
  console.log('click mouseUp')
}
onMounted( () => {
  window.addEventListener('mousemove', mouseMove);
  window.addEventListener('mouseup', mouseUp);
})
</script>