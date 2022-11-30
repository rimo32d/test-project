<template>
  <header class="header">
    <img
      src="/images/logo.svg"
      alt="">
    <h1>Vue.js ハンズオン</h1>
  </header>
  <main class="main">
    <div class="item">
      <div class="thumbnail">
        <img
          :src="item.image"
          alt="">
      </div>
      <div class="description">
        <h2>{{ item.name }}</h2>
        <p>{{ item.description }}</p>
        <span>¥<span class="price">{{ item.price }}</span></span>
      </div>
    </div>
  </main>
</template>
<script setup>
import { ref } from 'vue'
const item = ref({
  id: 1,
  name: 'アボカドディップバケット',
  description:
    '刻んだ野菜をアボカドと混ぜてディップに。こんがり焼いたバゲットとお召し上がりください。',
  price: 480,
  image: '/images/item1.jpg'
})
</script>