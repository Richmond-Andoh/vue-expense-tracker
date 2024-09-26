<template>
  <div class="container">
    <Header />
    <Balance :total="+total" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeletion"
    />
    <IncomeExpense :income="+income" :expenses="+expenses" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmission" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import Balance from "./components/Balance.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed } from "vue";
import { useToast } from "vue-toastification";

const transactions = ref([
  { id: 1, text: "First item", amount: 100.5 },
  { id: 2, text: "Second item", amount: 200.75 },
  { id: 3, text: "Third item", amount: -150.0 },
  { id: 4, text: "Fourth item", amount: 250.25 },
  { id: 5, text: "Fifth item", amount: 300.0 },
  { id: 6, text: "Sixth item", amount: 50.4 },
  //   { id: 7, text: "Seventh item", amount: 120.6 },
  //   { id: 8, text: "Eighth item", amount: 80.2 },
  //   { id: 9, text: "Ninth item", amount: 90.9 },
  //   { id: 10, text: "Tenth item", amount: 400.0 },
]);

const toast = useToast();

// GET TOTAL
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// GET INCOME
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// Helper function to generate unique ID

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};
const handleTransactionSubmission = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
};

const handleTransactionDeletion = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  toast.success("Transaction Deleted Successfully");
};
</script>
