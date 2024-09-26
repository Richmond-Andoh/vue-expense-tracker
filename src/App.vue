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

import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

const transactions = ref([]);

const toast = useToast();

onMounted(() => {
  const savedTransactions =
    JSON.parse(localStorage.getItem("transactions")) || [];
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

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

// SAVE TRANSACTION
const handleTransactionSubmission = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
  toast.success("Transaction Data Submitted Successfully");
};

const handleTransactionDeletion = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );

  localStorage.setItem("transactions", JSON.stringify(transactions.value));

  toast.success("Transaction Deleted Successfully");
};
</script>
