<script setup>
import { computed, ref } from "vue";

let id = 0;
const memos = ref([]);
const editId = ref(null);

const textBeforeEditing = computed(() => {
  const targetId = editId.value;
  const targetMemo = memos.value.find((memo) => {return memo.id === targetId})
  return targetMemo.text
});

let textBeingEdited = "";

const getInput = (input) => {
  textBeingEdited = input;
}

const addMemo = () => {
  memos.value.push({ id: ++id, text:"新規メモ"});
  editId.value = id;
}

const updateMemo = () => {
  const targetId = editId.value;
  let targetMemo = memos.value.find((memo) => {return memo.id === targetId});
  targetMemo.text = textBeingEdited;
  textBeingEdited = "";
}

</script>

<template>
  <div>
    <ul>
      <li v-for="memo in memos" :key="memo.id">
        <span>{{ memo.text }}</span>
      </li>
    </ul>

    <div>
      <button class="add-button" @click="addMemo">+</button>
    </div>

    <div v-if="editId" class="edit-area">
      <div class="edit-area-wrapper">
        <form>
          <textarea v-bind:value="textBeforeEditing" @input="getInput($event.target.value)"></textarea>
          <div class="button-area">
            <button @click.prevent="updateMemo">編集</button>
            <button>削除</button>
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
