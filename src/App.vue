<script setup>
import TrackerHeader from './components/TrackerHeader.vue'
import TrackerBalance from './components/TrackerBalance.vue'
import TrackerIncomeExpense from './components/TrackerIncomeExpense.vue'
import TrackerTransactionList from './components/TrackerTransactionList.vue'
import TrackerAddTransaction from './components/TrackerAddTransaction.vue'

import { ref, computed, onMounted } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()

const transactions = ref([])

onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem('transactions'))

  if (savedTransaction) {
    transactions.value = savedTransaction
  }
})

const totalAmount = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
})

const totalIncome = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})

const totalExpense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  })

  saveTransactionsToLocalStorage()

  toast.success('Операция добавлена!')
}

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)

  toast.success('Операция удалена')
}

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<template>
  <TrackerHeader />
  <div class="container">
    <TrackerBalance :totalAmount="+totalAmount" />
    <TrackerIncomeExpense :totalIncome="+totalIncome" :totalExpense="+totalExpense" />
    <TrackerTransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <TrackerAddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>
