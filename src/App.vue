<template>
  <member-modal :show="memberShow" @close="closeMember" @add-member="addMember"></member-modal>
  <payment-modal
    :show="paymentShow"
    :members="members"
    @close="closePayment"
    @add-payment="addPayment"
  >
  </payment-modal>
  <div class="container">
    <main>
      <Members v-if="toggleShow" :members="members" :payments="payments" @show-member="showMember">
      </Members>
      <Payments v-else :payments="payments" @del-payment="delPayment"></Payments>
    </main>
    <footer>
      <button class="share-payment" @click="toggle">{{ toggleName }}</button>
      <button class="add-payment" @click="showPayment">&#x2b;</button>
      <button class="end-share">結束分帳</button>
    </footer>
  </div>
</template>

<script setup>
import { v4 as uuidv4 } from 'uuid'

import Members from './components/Members.vue'
import memberModal from './components/AddMembers.vue'
import Payments from './components/Payments.vue'
import paymentModal from './components/AddPayments.vue'

import { ref, reactive, computed } from 'vue'

let toggleShow = ref(true)

let toggleName = computed(() => {
  return toggleShow.value ? '分帳款項' : '分帳成員'
})

function toggle() {
  toggleShow.value = !toggleShow.value
}

const members = reactive([])
let memberShow = ref(false)

function showMember() {
  memberShow.value = true
}

function closeMember() {
  memberShow.value = false
}

function addMember(name) {
  const member = {
    id: uuidv4(),
    name,
  }
  members.push(member)
}

const payments = reactive([])
let paymentShow = ref(false)

function showPayment() {
  paymentShow.value = true
}

function closePayment() {
  paymentShow.value = false
}

function addPayment({ paymentName, paymentPrice, payerId }) {
  let payer = members.find((item) => item.id === payerId)
  const payment = {
    id: uuidv4(),
    paymentName,
    paymentPrice,
    payer: payer.name,
    payerId
  }
  payments.push(payment)
}

function delPayment(paymentId) {
  const targetIndex = payments.findIndex((item) => item.id === paymentId)
  payments.splice(targetIndex, 1)
}
</script>

<style>
.container {
  position: relative;
  display: block;
  width: 300px;
  background-color: var(--main-background);
  height: 600px;
  border-radius: 10px;
  margin: auto;
  overflow: hidden;
}

main {
  margin-top: 20px;
  height: 90%;
  overflow: auto;
}

main::-webkit-scrollbar {
  width: 7px;
}

main::-webkit-scrollbar-button {
  background: transparent;
  border-radius: 4px;
}

main::-webkit-scrollbar-track-piece {
  background: transparent;
}

main::-webkit-scrollbar-thumb {
  border-radius: 4px;
  background-color: rgba(0, 0, 0, 0.4);
  border: 1px solid slategrey;
}

main::-webkit-scrollbar-track {
  box-shadow: transparent;
}

footer {
  position: absolute;
  display: flex;
  justify-content: space-between;
  background-color: var(--main-background);
  left: 0;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  height: 50px;
  border-top: 2px solid #000;
  z-index: 2;
}

footer button {
  cursor: pointer;
  border: none;
  background-color: var(--main-background);
  font-size: 1.2rem;
  font-weight: bold;
}

.share-payment,
.end-share {
  transform: scale(0.9);
}

.share-payment:hover,
.end-share:hover {
  transform: scale(1);
}

.add-payment {
  position: absolute;
  left: 50%;
  top: 0;
  transform: translate(-50%, -50%) scale(0.9);
  background-color: var(--main-background);
  border: 2px solid #000;
  width: 70px;
  height: 70px;
  border-radius: 50%;
  font-size: 3.5rem;
  font-weight: normal;
}

.add-payment:hover {
  transform: translate(-50%, -50%) scale(1);
}
</style>
