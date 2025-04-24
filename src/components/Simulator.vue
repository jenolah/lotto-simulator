<script setup>
import { ref, onUnmounted } from 'vue'
import Metrics from './Metrics.vue'

let idInterval = ref(null)
const userNumbers = ref([1, 2, 3, 4, 5])
const winningNumbers = ref(new Set([1, 2, 3, 4, 5]))
const intervalTime = ref(1000)
const numberOfTickets = ref(0)
const matches = ref({
  2: 0,
  3: 0,
  4: 0,
  5: 0,
})
const inputString = ref('')
const isRandom = ref(false)

const resetInterval = () => {
  if (idInterval.value) clearInterval(idInterval.value)
  idInterval.value = setInterval(processLotteryRound, intervalTime.value)
}

const handleSliderChange = () => {
  if (idInterval.value) resetInterval()
}

const stopInterval = () => {
  clearInterval(idInterval.value)
  idInterval.value = null
}

const getRandomNumbers = (min, max, count) => {
  const numbers = new Set()

  while (numbers.size < count) {
    const maxInt = 255
    const randomFloat = self.crypto.getRandomValues(new Uint8Array(1))[0] / (maxInt + 1)
    const randomNumberInRange = Math.floor(randomFloat * (max - min + 1)) + min
    numbers.add(randomNumberInRange)
  }
  return numbers
}

const startSimulation = () => {
  if (isRandom.value) {
    userNumbers.value = Array.from(getRandomNumbers(1, 90, 5))
  } else {
    const raw = inputString.value || ''
    const numbersArray = raw
      .split(',')
      .map((n) => parseInt(n.trim(), 10))
      .filter((n) => !isNaN(n))

    userNumbers.value = numbersArray && new Set(numbersArray).size === 5 ? numbersArray : null
  }

  userNumbers.value ? resetInterval() : alert('Please enter 5 unique numbers between 1 and 90')
}

const processLotteryRound = () => {
  const randomNumbers = getRandomNumbers(1, 90, 5)
  winningNumbers.value = randomNumbers
  const userNumbersSet = new Set(userNumbers.value)
  const intersection = randomNumbers.intersection(userNumbersSet)
  if (intersection.size >= 2) {
    matches.value[intersection.size] = matches.value[intersection.size] + 1
  }
  numberOfTickets.value += 1

  if (matches.value[5] > 0) {
    stopInterval()
    alert('You won the lottery!')
  }
}

onUnmounted(() => {
  stopInterval()
})
</script>

<template>
  <div class="container">
    Your numbers separated by commas:
    <input
      type="text"
      :disabled="isRandom"
      v-model="inputString"
      placeholder="e.g. 4, 8, 15, 16, 23"
    />
    <div>Play with random numbers:<input type="checkbox" v-model="isRandom" /></div>
    <button :disabled="idInterval" @click="startSimulation">START</button>

    <button @click="stopInterval">STOP</button>

    <h2 class="title">Result</h2>
    <div class="grid">
      <p>Your numbers:</p>

      <div class="number-list">
        <span v-for="number in userNumbers ? userNumbers : [1, 2, 3, 4, 5]" class="number">
          {{ number }}
        </span>
      </div>
      <p>Winning numbers:</p>

      <div class="number-list">
        <span v-for="number in Array.from(winningNumbers)" class="number">
          {{ number }}
        </span>
      </div>
    </div>
    <Metrics :numberOfTickets="numberOfTickets" />
    <div class="matches">
      <div class="match-card" v-for="(match, index) in matches">
        <p class="text-small">{{ index }} matches</p>
        <p class="bold">{{ match }}</p>
      </div>
    </div>
    <div class="slidercontainer">
      Speed
      <input
        class="slider"
        type="range"
        v-model="intervalTime"
        @input="handleSliderChange"
        min="1"
        max="1000"
        step="1"
      />
      <span>{{ intervalTime }} ms</span>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@use '@/assets/base';

.container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin: auto;
  max-width: 700px;
  background-color: white;
  padding: 1rem;
  border-radius: 2rem;
  box-shadow: 2px 2px 10px base.$shadow-color;
}
.grid {
  display: grid;
  grid-template-columns: max-content auto;
  align-items: center;
  gap: 1rem;
  @media screen and (max-width: 600px) {
    grid-template-columns: 1fr;
  }
}

.metrics {
  font-size: 1.2em;
  padding: 1rem;
  border-radius: 10px;
  background-color: base.$primary-color;
  color: white;
}

.matches {
  display: grid;
  justify-content: center;
  align-items: center;
  margin-top: 1rem;
  padding: 1rem;
  grid-template-columns: repeat(4, 1fr);

  @media screen and (max-width: 600px) {
    grid-template-columns: repeat(2, 1fr);
  }
}

.match-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  background-color: white;
  border: 1px solid base.$secondary-color;
  color: base.$text-color-dark;
  padding: 0.5rem;
  box-shadow: 0 0 5px base.$shadow-color;
  border-radius: 0;

  @media screen and (min-width: 600px) {
    &:first-child {
      border-top-left-radius: 8px;
      border-bottom-left-radius: 8px;
    }

    &:last-child {
      border-top-right-radius: 8px;
      border-bottom-right-radius: 8px;
    }
  }

  @media screen and (max-width: 600px) {
    &:first-child {
      border-top-left-radius: 8px;
    }
    &:nth-child(2) {
      border-top-right-radius: 8px;
    }
    &:nth-last-child(2) {
      border-bottom-left-radius: 8px;
    }
    &:last-child {
      border-bottom-right-radius: 8px;
    }
  }
}

.number-list {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  align-items: center;
}

.number {
  background-color: white;
  border: 1px solid base.$primary-color;
  border-radius: 10px;
  color: base.$text-color-dark;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 2rem;
  height: 2rem;
  font-size: 1rem;
  font-weight: 400;
  box-shadow: 0 0 5px base.$shadow-color;
}
</style>
