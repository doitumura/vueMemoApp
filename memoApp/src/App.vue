<script setup>
import { ref } from "vue";

let id = 0;
const memos = ref([]);
const edit = ref(false);

</script>

<template>
  <div>
    <ul>
      <li v-for="memo in memos" :key="memo.id">
        <span>{{ memo.text }}</span>
      </li>
    </ul>

    <div>
      <button class="add-button">+</button>
    </div>

    <div v-if="edit" class="edit-area">
      <div class="edit-area-wrapper">
        <textarea></textarea>
        <div class="button-area">
          <button>編集</button>
          <button>削除</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-size: 70px;
}

.add-button {
  font-size: 20px;
}

.edit-area {
  position: fixed;
  right: 20px;
  top: 100px;
}

.edit-area-wrapper {
  display: flex;
  flex-direction: column;
}

textarea {
  width: 500px;
  height: 500px;
}

</style>
