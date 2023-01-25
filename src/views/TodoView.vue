<template>
  <div id="trello" class="l-flex" style="align-items: flex-start">
    <div v-for="(list, index) in lists" :key="index" class="c-list">
      <div class="c-list--title">{{ list.name }}</div>
      <div
        v-for="(card, index) in list.cards"
        :key="index"
        class="c-card sortable"
        @mousedown="mousedown"
        draggable="false"
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
    <div ref="drag"></div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
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
      },
      {
        id: 2,
        name: "仕様書の作成",
        description: "webサイトの仕様書作成",
        user_name: "鈴木",
      },
      {
        id: 3,
        name: "仕様書の作成2",
        description: "webサイトの仕様書作成",
        user_name: "鈴木",
      },
    ],
  },
  // {
  //   id: 2,
  //   name: "作業中",
  //   cards: [
  //     {
  //       id: 4,
  //       name: "見積もりの作成",
  //       description: "",
  //       user_name: "山田",
  //     },
  //   ],
  // },
  // {
  //   id: 3,
  //   name: "完了",
  //   cards: [
  //     {
  //       id: 5,
  //       name: "B社への支払い",
  //       description: "経理への連絡を忘れないように",
  //       user_name: "鈴木",
  //     },
  //   ],
  // },
]);

let element = ref("");
let dragging = ref(false);

let pageX = ref(0);
let pageY = ref(0);
let cardTop = ref(0);
let cardLeft = ref(0);
let cardHeight = ref(0);

let placeHolder = ref("");
let draggingElement = ref("");
let drag = ref("");

let firstDrag = ref(true);

let mousedown = (e) => {
  // クリック時
  dragging.value = true;
  element.value = e.target;
  pageX.value = e.pageX;
  pageY.value = e.pageY;
  cardTop.value = element.value.getBoundingClientRect().top;
  cardLeft.value = element.value.getBoundingClientRect().left;
  cardHeight.value = element.value.getBoundingClientRect().height;
  firstDrag.value = true;
};
let mouseMove = (e) => {
  console.log(e.pageY)
  // マウス移動時
  if (dragging.value) {
    if (firstDrag.value) {
      placeHolder.value = document.createElement("div");
      placeHolder.value.style.height = `${cardHeight.value}px`;
      placeHolder.value.classList.add("c-card", "c-card--placeHolder");
      element.value.parentNode.insertBefore(
        placeHolder.value,
        element.value.nextSibling
      );
      // ドラッグ要素のクローンを作成
      draggingElement.value = element.value.cloneNode(true);
      // 元の位置にあった要素を非表示
      element.value.style.display = "none";
      draggingElement.value.style.position = "absolute";
      drag.value.appendChild(draggingElement.value);
      draggingElement.value.style.top = `${cardTop.value}px`;
      draggingElement.value.style.left = `${cardLeft.value}px`;
      // 要素を傾ける
      draggingElement.value.classList.add("c-card--active");
      firstDrag.value = false;
    }
    // 他タスク情報を取得
    const lists = document.querySelectorAll(".c-list");
    lists.forEach(list => {
      const sortables = [...list.querySelectorAll('.sortable')].filter(card => card.style.display != "none")
      const belowElement = sortables.reduce((closestElement, sortable) => {
        const taskElementBox = sortable.getBoundingClientRect()
        const offsetY = e.pageY - (taskElementBox.top + taskElementBox.height / 2);
        console.log(offsetY)
        console.log(closestElement.offsetY)
        if(offsetY < 0 && offsetY > closestElement.offsetY) {
          return {
            offsetY: offsetY,
            element: sortable,
          }
        } else {
          return closestElement;
        }
      }, { offsetY: Number.NEGATIVE_INFINITY}).element
      placeHolder.value.remove();
      placeHolder.value = document.createElement("div");
      placeHolder.value.style.height = `${cardHeight.value}px`
      placeHolder.value.classList.add("c-card", "c-card--placeHolder");
      if (belowElement == undefined) {
        list.appendChild(placeHolder.value)
      } else {
        list.insertBefore(placeHolder.value, belowElement)
      }
    })
    let moveX = e.pageX - pageX.value + cardLeft.value;
    let moveY = e.pageY - pageY.value + cardTop.value;
    draggingElement.value.style.top = moveY + "px";
    draggingElement.value.style.left = moveX + "px";
  }
};
let mouseUp = () => {
  // マウスを離した時
  placeHolder.value.remove();
  draggingElement.value.remove();
  element.value.style.display = "block";
  dragging.value = false;
  // console.log("click mouseUp");
};

onMounted(() => {
  window.addEventListener("mousemove", mouseMove);
  window.addEventListener("mouseup", mouseUp);
});
</script>