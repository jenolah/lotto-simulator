<script setup>
const { numberOfTickets } = defineProps(['numberOfTickets'])
</script>

<template>
  <div class="metrics grid">
    <div>Number of tickets:</div>
    <div>{{ numberOfTickets }}</div>
    <div>Years spent:</div>
    <div>{{ Math.round(numberOfTickets / 52) }}</div>
    <div>Cost of tickets:</div>
    <div>{{ numberOfTickets * 300 }} HUF</div>
  </div>
</template>

<style lang="scss" scoped>
@use '@/assets/base';
</style>
