<template>
  <div class="container">
    <Header />
    <Balance :total />
    <TransactionList :transactions="transactions" />
    <IncomeExpense />
    <AddTransaction />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import Balance from "./components/Balance.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed } from "vue";

const transactions = ref([
  { id: 1, text: "First item", amount: 100.5 },
  { id: 2, text: "Second item", amount: 200.75 },
  { id: 3, text: "Third item", amount: -150.0 },
  { id: 4, text: "Fourth item", amount: 250.25 },
  { id: 5, text: "Fifth item", amount: -300.0 },
  { id: 6, text: "Sixth item", amount: 50.4 },
  //   { id: 7, text: "Seventh item", amount: 120.6 },
  //   { id: 8, text: "Eighth item", amount: 80.2 },
  //   { id: 9, text: "Ninth item", amount: 90.9 },
  //   { id: 10, text: "Tenth item", amount: 400.0 },
]);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});
</script>
