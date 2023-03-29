<script setup>
import { ref } from "vue";

let id = 0;
const editId = ref(null);
const memos = ref([]);

let textBeforeEditing = "";
let textBeingEdited = "";
let edited = false;

const getInput = (input) => {
  textBeingEdited = input;
  edited = true;
}

const addMemo = () => {
  memos.value.push({ id: ++id, text:"新規メモ"});
  textBeforeEditing = "新規メモ";
  editId.value = id;
}

const editMemo = (memo) => {
  textBeforeEditing = memo.text;
  editId.value = memo.id;
}

const updateMemo = () => {
  if(!edited) textBeingEdited = textBeforeEditing;
  memos.value.find((memo) => {return memo.id === editId.value}).text = textBeingEdited;

  textBeingEdited = "";
  edited = false;
  editId.value = null;
}

const getFirstLine = (text) => {
  return text.split(/\r\n|\n/)[0];
}

</script>

<template>
  <div>
    <ul>
      <li v-for="memo in memos" :key="memo.id">
        <span @click="editMemo(memo)">{{ getFirstLine(memo.text) }}</span>
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

span {
  cursor: pointer;
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
