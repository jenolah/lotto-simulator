<script setup>
const { numberOfTickets, hasWon } = defineProps(['numberOfTickets', 'hasWon'])

const formatNumber = (number) => {
  return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ')
}
</script>

<template>
  <div class="metrics grid">
    <div class="bold">Number of tickets:</div>
    <div class="bold">{{ formatNumber(numberOfTickets) }}</div>
    <div :class="{ won: hasWon }">Years spent:</div>
    <div :class="{ won: hasWon }">{{ formatNumber(Math.round(numberOfTickets / 52)) }}</div>
    <div>Cost of tickets:</div>
    <div>{{ formatNumber(numberOfTickets * 300) }} HUF</div>
  </div>
</template>

<style lang="scss" scoped>
.won {
  font-size: 2rem;
  font-weight: 800;
  background: linear-gradient(
    to right,
    #ef5350,
    #f48fb1,
    #7e57c2,
    #2196f3,
    #26c6da,
    #43a047,
    #eeff41,
    #f9a825,
    #ff5722
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
