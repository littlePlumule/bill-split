<template>
  <div class="overlay" v-if="show" @click.self="close"></div>
  <div class="modal" v-if="show">
    <form @submit.prevent="addMember">
      <input type="text" placeholder="請輸入成員名稱" v-model.trim="name" />
      <button class="add" type="submit">新增</button>
      <button class="cancel" type="button" @click="close">關閉</button>
      <p v-if="showError">不可為空、名稱限12個字內</p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps(['show'])
const emit = defineEmits(['close', 'add-member'])

let name = ref('')
let showError = ref(false)

function close() {
  showError.value = false
  name.value = ''
  emit('close')
}

function addMember() {
  if (name.value === '' || name.value.length > 12) {
    showError.value = true
    return
  }
  emit('add-member', name.value)
  close()
}
</script>

<style scoped>
.overlay {
  position: fixed;
  height: 100%;
  width: 100%;
  background-color: var(--overlay);
  z-index: 3;
}

.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 280px;
  background-color: var(--modal);
  border-radius: 8px;
  z-index: 4;
  padding: 15px;
}

input {
  display: block;
  width: 100%;
  border: none;
  outline: none;
  font-size: 1.2rem;
  padding: 4px;
  border-radius: 5px;
}

button {
  margin-top: 10px;
  width: 50%;
  font-size: 1.4rem;
  padding: 5px 0;
  cursor: pointer;
  border: none;
  outline: none;
  color: #fff;
}

.add {
  background-color: var(--add);
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
}

.add:hover {
  background-color: var(--hover);
}

.cancel {
  background-color: var(--no-one);
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
}

.cancel:hover {
  background-color: var(--cancel);
}

p {
  background-color: var(--error);
  color: #fff;
  border-radius: 8px;
  font-size: 1rem;
  padding: 2px 0;
  font-weight: normal;
  text-align: center;
  margin-top: 10px;
}
</style>
