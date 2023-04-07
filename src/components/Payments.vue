<template>
  <ul v-if="payments.length !== 0">
    <li v-for="payment of payments" :key="payment.id" v-cloak>
      <div class="title">
        <h2>{{ payment.paymentName }}</h2>
        <button class="delete" @click="delPayment(payment.id)">&#x2716;</button>
      </div>
      <div class="description">
        <p class="total">{{ payment.paymentPrice }}</p>
        <p class="payer">{{ payment.payer }}</p>
      </div>
    </li>
  </ul>
  <div class="no-payment" v-else>目前無支出款項</div>
</template>

<script setup>
const props = defineProps(['payments'])
const emit = defineEmits(['del-payment'])

function delPayment(id) {
  emit('del-payment', id)
}
</script>

<style scoped>
ul {
  padding: 0 20px;
}

li {
  border-bottom: 1px solid #000;
}

li + li {
  margin-top: 20px;
}

li:last-child {
  margin-bottom: 50px;
}

.title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4px;
}

h2 {
  font-size: 1.2rem;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.delete {
  border: none;
  background-color: transparent;
  font-size: 1.2rem;
  color: var(--no-one);
  cursor: pointer;
  transition: color 0.2s;
}

.delete:hover {
  color: var(--error);
}

.description {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.total,
.payer {
  font-weight: 600;
}

.total::before {
  content: '總額:NT$';
  font-size: 0.8rem;
  font-weight: normal;
  white-space: nowrap;
}

.payer {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-left: 15px;
}

.payer::before {
  content: '付款者:';
  font-size: 0.8rem;
  font-weight: normal;
  white-space: nowrap;
}

.no-payment {
  font-size: 24px;
  color: var(--no-one);
  margin: 0 auto;
  position: absolute;
  white-space: nowrap;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
