<template>
  <ul v-if="members.length !== 0">
    <li v-for="member of members" :key="member.id" v-cloak>
      <p>{{ member.name }}</p>
      <div class="price receivables" v-if="resultPrice(member) > 0">
        <span>應收NT$ </span>{{ resultPrice(member) }}
      </div>
      <div class="price payable" v-else-if="resultPrice(member) < 0">
        <span>應付NT$ </span>{{ resultPrice(member) }}
      </div>
      <div class="price" v-else="resultPrice(member)">
        <span>NT$ </span>{{ resultPrice(member) }}
      </div>
    </li>
  </ul>
  <div class="no-member" v-else>目前無成員</div>
  <button class="add-member" @click="showMember">&#x2b; 新增成員</button>
</template>

<script setup>
const props = defineProps(['members', 'payments'])
const emit = defineEmits(['show-member'])

function showMember() {
  emit('show-member')
}

function resultPrice(member) {
  let totalPrice = 0
  let totalPay = 0
  for (let payment of props.payments) {
    if (payment.payerId === member.id) {
      totalPay += payment.paymentPrice
    }
    totalPrice += payment.paymentPrice
  }
  let result = (totalPay - totalPrice / props.members.length).toFixed(2)
  return result
}
</script>

<style scoped>
ul {
  padding: 0 20px;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #000;
}

li + li {
  margin-top: 20px;
}

.price,
p {
  font-size: 1.2rem;
}

.price {
  flex-shrink: 0;
  font-weight: bold;
}

.price span {
  font-size: 0.8rem;
  font-weight: normal;
}

.receivables {
  color: var(--receivables);
}

.payable {
  color: var(--payable);
}

.no-member {
  font-size: 24px;
  color: var(--no-one);
  white-space: nowrap;
  margin: 0 auto;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.add-member {
  display: block;
  width: 80%;
  border: 2px solid #000;
  border-radius: 50px;
  padding: 5px 0;
  font-size: 24px;
  font-weight: bold;
  cursor: pointer;
  background-color: transparent;
  margin: 20px auto 50px;
  transform: scale(0.9);
}

.add-member:hover {
  transform: scale(1);
}
</style>
