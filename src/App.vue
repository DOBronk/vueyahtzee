<script setup>
import {ref} from 'vue';
import ScoreTable from './components/ScoreTable.vue'
import ThrownDice from './components/ThrownDice.vue'

// Dice array filled with example values
const arrDice = ref([2,2,3,4,5]);
</script>

<template>
  <ThrownDice v-model="arrDice" /><br>
  <ScoreTable v-model="arrDice" />
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
