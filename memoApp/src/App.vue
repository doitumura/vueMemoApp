<script setup>
import { ref } from "vue";

let id = 0;
const newMemo = ref("");
const memos = ref([]);

const addMemo = () => {
  memos.value.push({ id: id++, text: newMemo.value})
}
</script>

<template>
  <div>
    <h1>メモアプリ</h1>

    <div>
      <h2>メモを追加する</h2>
      <form @submit.prevent="addMemo">
        <input v-model="newMemo" />
        <button>追加</button>
      </form>
    </div>

    <div>
      <h2 v-if="memos.length > 0">メモ一覧</h2>
      <ul>
        <li v-for="memo in memos" :key="memo.id">
          <span>{{ memo.text }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-size: 70px;
}
</style>
