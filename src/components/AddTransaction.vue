<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';
const text = ref('');
const amount = ref('');

const emit = defineEmits(['addTransaction']);

const toast = useToast();

const onSubmit = () => {
  if (text.value.trim() === '' || amount.value.trim() === '') {
    toast.error('Please add a text and amount');
    return;
  }
  const newTransaction = {
    id: Math.floor(Math.random() * 100000000),
    text: text.value,
    amount: +amount.value,
  };
  emit('addTransaction', newTransaction);
  text.value = '';
  amount.value = '';
};
</script>
