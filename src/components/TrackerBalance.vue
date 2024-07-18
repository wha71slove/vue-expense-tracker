<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  totalAmount: {
    type: Number,
    required: true
  }
})
</script>

<template>
  <h4>Ваш баланс</h4>
  <h1 id="balance">{{ totalAmount }} ₽</h1>
</template>
