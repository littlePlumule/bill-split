<template>
  <div class="overlay" v-if="show" @click.self="close"></div>
  <div class="modal" v-if="show">
    <form @submit.prevent="addPayment">
      <input type="text" placeholder="請輸入款項名稱" v-model.trim="paymentName" />
      <input type="number" placeholder="請輸入款項金額" v-model="paymentPrice" />
      <select v-model="payer">
        <option disabled value="">選擇付款人</option>
        <option v-for="member of members" :key="member.id" :value="member.id" v-cloak>
          {{ member.name }}
        </option>
      </select>
      <button class="add" type="submit">新增</button>
      <button class="cancel" type="button" @click="close">關閉</button>
      <p v-if="showError">款項名稱不得為空且限20個字內、款項金額限100萬內且不能為負數</p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps(['show', 'members'])
const emit = defineEmits(['close', 'add-payment'])

let paymentName = ref('')
let paymentPrice = ref(null)
let payer = ref('')
let showError = ref(false)

function close() {
  paymentName.value = ''
  paymentPrice.value = null
  payer.value = ''
  showError.value = false
  emit('close')
}

function addPayment() {
  if (!paymentName.value || paymentPrice.value <= 0 || paymentPrice.value >= 1000000 || !payer.value) {
    showError.value = true
    return
  }
  const item = {
    paymentName: paymentName.value,
    paymentPrice: paymentPrice.value,
    payerId: payer.value
  }
  emit('add-payment', item)
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

input,
select {
  display: block;
  width: 100%;
  border: none;
  outline: none;
  font-size: 1.2rem;
  padding: 4px;
  border-radius: 5px;
}

input:nth-child(2) {
  margin: 10px 0;
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
