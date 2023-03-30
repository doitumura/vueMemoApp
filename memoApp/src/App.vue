<script setup>
import { ref, watch } from "vue";

let id = localStorage.getItem("id") || 1;
const editId = ref(null);
const editText = ref("");
const memos = ref(JSON.parse(localStorage.getItem("memos")) || []);

watch(
  memos,
  (newMemos) => {
    const memosJson = JSON.stringify(newMemos);
    localStorage.setItem("memos", memosJson);
    localStorage.setItem("id", id);
  },
  { deep: true }
);

const addMemo = () => {
  memos.value.push({ id: ++id, text: "新規メモ" });
  editText.value = "新規メモ";
  editId.value = id;
};

const editMemo = (memo) => {
  editText.value = memo.text;
  editId.value = memo.id;
};

const updateMemo = () => {
  memos.value.find((memo) => {
    return memo.id === editId.value;
  }).text = editText.value;
  editText.value = "";
  editId.value = null;
};

const deleteMemo = () => {
  memos.value = memos.value.filter((memo) => {
    return memo.id !== editId.value;
  });
  editId.value = null;
};

const getFirstLine = (text) => {
  return text.split(/\r\n|\n/)[0];
};
</script>

<template>
  <div>
    <ul>
      <li
        v-for="memo in memos"
        :key="memo.id"
        :class="[memo.id === editId ? 'clicked' : '']"
        @click="editMemo(memo)"
      >
        <span>{{ getFirstLine(memo.text) }}</span>
      </li>
    </ul>

    <div>
      <button class="add-button" @click="addMemo">+</button>
    </div>

    <div v-if="editId" class="edit-area">
      <div class="edit-area-wrapper">
        <form>
          <textarea v-model="editText"></textarea>
          <div class="button-area">
            <button @click.prevent="updateMemo">編集</button>
            <button @click.prevent="deleteMemo">削除</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-size: 70px;
}

li {
  cursor: pointer;
}

.clicked {
  color: red;
}

.add-button {
  font-size: 20px;
}

.edit-area {
  position: fixed;
  left: 600px;
  top: 40px;
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
