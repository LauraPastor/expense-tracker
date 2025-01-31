<template>
  <div class="container">
    <Header> </Header>
    <Balance id="balance" :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @delete-transaction="handleDeleteTransaction" />
    <AddTransaction @add-transaction="handleAddTransaction" />
  </div>
</template>
<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { useToast } from 'vue-toastification';
import { ref, computed, onMounted } from 'vue';

const toast = useToast();

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const transactions = ref([
  { id: 1, text: 'Flower', amount: -20 },
  { id: 2, text: 'Salary', amount: 300 },
  { id: 3, text: 'Book', amount: -10 },
  { id: 4, text: 'Camera', amount: 150 },
]);

const total = computed(() => {
  return transactions.value.reduce(
    (acc, transaction) => (acc += transaction.amount),
    0
  );
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => (acc += transaction.amount), 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => (acc += transaction.amount), 0)
    .toFixed(2);
});

const handleAddTransaction = (newTransaction) => {
  transactions.value.push(newTransaction);
  saveTransactions();
  toast.success('Transaction added');
};

const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  toast.success('Transaction deleted');
};
const saveTransactions = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};
</script>
