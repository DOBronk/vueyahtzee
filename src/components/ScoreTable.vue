<script setup>
// Our score table for a visual representation of scores

// TODO: Nog ff implementeren: three and four of a kind optellen + kleine bug (grote straat slaat aan op klein en groot)

import { computed } from 'vue'
const diceNumbers = ["Enen", "Tweeën", "Drieën", "Vieren", "Vijfen", "Zessen"];
const mod = defineModel();

// Total score of all roled dices
const totalScore = computed(() => { return mod.value.reduce((a, b) => a + b, 0);  });

// Three and four of a kind
const threesomes = computed(() => {  return totals.filter((total) => total.value === 3).length;});
const foursomes = computed(() => {  return totals.filter((total) => total.value >= 4).length;});

// Full house
const fullhouse = computed(() => {
  return totals.filter((total) => total.value === 2).length > 0 && totals.filter((total) => total.value === 3).length > 0 ? 1 : 0;
});

// Streets
const smallstreet = computed(() => { return findStreets(true) - findStreets(false); });
const bigstreet = computed(() => { return findStreets(false); });

// Topscore
const topscore = computed(() => {
  let first = mod.value[0];
  for (let i = 1; i < mod.value.length; i++) {
    if(mod.value[i] !== first) {
      return 0;
    }
  }
  return 1;
});

const bottomhalf = computed(() => {
  let beginscore = totalScore;

  if(fullhouse > 0)
  {
    beginscore += 25;
  }
  if(smallstreet > 0)
  {
    beginscore += 30;
  }
  if(bigstreet > 0)
  {
    beginscore += 40;
  }
  if(topscore > 0)
  {
    beginscore += 50;
  }

  return beginscore;
});

// Dynamically create computed values and dump them in an array!
const totals = [];

for(let i = 0; i < 6; i++)
{
  totals.push(computed(() => {  return getDice(i); }))
}

function findStreets(small)
{
  // Failures, allow the first to fail, terminate and return 0 on second failure. Remove this for big street.
  let failures = 0;
  let allowfail = (small.value === true) ? 1: 0;

  for(let i = 1; i < 5; i++) {
    if(allowfail > 0 && failures > allowfail)
    {
      break;
    }
    if(mod.value[i - 1] !== mod.value[i] + 1)
    {
      failures++;
    }
    if(i === 4) return 1; // TODO: Aanpassen aan grote en kleine straat
  }
  // If not found rinse and repeat backwards
  failures = 0;

  for(let i = 4; i > 0; i--) {
    if(allowfail > 0 && failures > allowfail)
    {
      return 0;
    }
    if(mod.value[i - 1] !== mod.value[i] + 1)
    {
      failures++;
    }
  }

  // Guess we found something
  return 1;
}
function getDice(diceNumber) {
  return mod.value.filter((dice) => dice === diceNumber).length;
}
</script>

<template>
  <h1>Score blok</h1><br>
  <table>
    <tbody>
      <tr>
        <td><b>Deel 1</b></td>
        <td v-for="index in 6" :key="index">{{index}}e spel</td>
      </tr>
    <tr v-for="(val,index) in 7" :key="index">
      <td>{{diceNumbers[index - 1]}}</td>
      <td>{{totals[index-1]}}</td>
    </tr>
      <tr><td><b>Totaal</b></td><td>{{totalScore}}</td></tr>
    </tbody>
  </table>
  <br>
  <table>
    <tbody>
    <tr>
      <td colspan="2">Deel 2</td>
    </tr>
    <tr>
      <td>Three of kind: {{threesomes}}</td>
      <td>Carré: {{foursomes}}</td>
      <td>Full house: {{fullhouse}}</td>
      <td>Kleine straat: {{smallstreet}}</td>
      <td>Grote straat: {{bigstreet}}</td>
      <td>Topscore: {{topscore}}</td>
      <td>Change: {{totalScore}}</td>

      <td>Totaal deel 1: {{totalScore}}</td>
      <td>Totaal deel 2: {{bottomhalf}}</td>
      <td></td>
    </tr>
    </tbody>
  </table>
</template>

<style scoped>

</style>