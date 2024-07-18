<script setup>
import { ref } from 'vue'
import { useToast } from 'vue-toastification'

const text = ref('')
const amount = ref('')

const emit = defineEmits(['transactionSubmitted'])

const toast = useToast()

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Оба поля должны быть заполнены!')
    return
  }
  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
  }

  emit('transactionSubmitted', transactionData)

  text.value = ''
  amount.value = ''
}
</script>

<template>
  <h3>Добавить новую запись</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Описание</label>
      <input type="text" id="text" v-model="text" placeholder="Введите описание..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Сумма <br />
        (трата пишется со знаком минус)</label
      >
      <input type="number" id="amount" v-model="amount" placeholder="Введите сумму..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
