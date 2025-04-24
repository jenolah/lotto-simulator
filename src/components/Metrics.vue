<script setup>
const { numberOfTickets } = defineProps(['numberOfTickets'])
const formatNumber = (number) => {
  return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ')
}
</script>

<template>
  <div class="metrics grid">
    <div>Number of tickets:</div>
    <div>{{ numberOfTickets }}</div>
    <div class="bold">Number of tickets:</div>
    <div class="bold">{{ formatNumber(numberOfTickets) }}</div>
    <div>Years spent:</div>
    <div>{{ formatNumber(Math.round(numberOfTickets / 52)) }}</div>
    <div>Cost of tickets:</div>
    <div>{{ formatNumber(numberOfTickets * 300) }} HUF</div>
  </div>
</template>

<style lang="scss" scoped>
@use '@/assets/base';
</style>
