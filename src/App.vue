<script setup>
import Simulator from './components/Simulator.vue'
</script>

<template>
  <header>
    <img src="/plus.svg" alt="Logo" />
    <h1 class="title">Lottery Simulator</h1>
  </header>

  <main>
    <Simulator />
  </main>
</template>

<style lang="scss" scoped>
@use '@/assets/base';

header {
  display: flex;
  gap: 1rem;
  color: white;
  background: linear-gradient(90deg, base.$primary-color 0%, base.$secondary-color 100%);
  padding: 1rem;
  margin-bottom: 1rem;
}
</style>
