<script setup>
// Our score table for a visual representation of scores

// TODO: Nog ff implementeren: three and four of a kind optellen

import { computed } from 'vue'
const diceNumbers = ["Enen", "Tweeën", "Drieën", "Vieren", "Vijfen", "Zessen"];
const mod = defineModel();

// Total score of all roled dices
const totalScore = computed(() => { return mod.value.reduce((a, b) => a + b, 0);  });

// Three and four of a kind
const threesomes = computed(() => {  return totals.filter((total) => total.value === 3).length;});
const foursomes = computed(() => {  return totals.filter((total) => total.value >= 4).length;});

// Full house
const fullHouse = computed(() => {
  return totals.filter((total) => total.value === 2).length > 0 && totals.filter((total) => total.value === 3).length > 0 ? 1 : 0;
});

// Streets
const bigStreet = computed(() => { return findStreets(false); });
const smallStreet = computed(() => { return findStreets(true); });


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

const bottomHalf = computed(() => {
  let beginScore = totalScore.value;

  if(threesomes.value > 0 || foursomes.value > 0) {
    beginScore *= 2;
  }
  if(fullHouse.value > 0)
  {
    beginScore += 25;
  }
  if(smallStreet.value > 0)
  {
    beginScore += 30;
  }
  if(bigStreet.value > 0)
  {
    beginScore += 40;
  }
  if(topscore.value > 0)
  {
    beginScore += 50;
  }

  return beginScore;
});

// Dynamically create computed values and dump them in an array!
const totals = [];

for(let i = 0; i < 6; i++)
{
  totals.push(computed(() => {  return getDice(i + 1); }))
}

function findStreets(small)
{
  // Sort the array, then find out if we have consecutive numbers
  let sortedArray = mod.value.toSorted();

  if(!small)
  {
    // If not searching for small streets, check if all numbers are consecutive
    for(let i = 0; i < sortedArray.length - 1; i++)
    {
      if(sortedArray[i] !== sortedArray[i + 1] - 1) {
        return 0;
      }
    }
  }
  else
  {
    // Allow for a single failure whilest checking for small streets
    let failures = 0;

    for(let i = 0; i < sortedArray.length - 1; i++)
    {
      if(sortedArray[i] !== sortedArray[i + 1] - 1) {
        failures += 1;
      }
      if(failures > 1)
      {
        return 0;
      }
    }
  }

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
      <td>Full house: {{fullHouse}}</td>
      <td>Kleine straat: {{smallStreet}}</td>
      <td>Grote straat: {{bigStreet}}</td>
      <td>Topscore: {{topscore}}</td>
      <td>Change: {{totalScore}}</td>

      <td>Totaal deel 1: {{totalScore}}</td>
      <td>Totaal deel 2: {{bottomHalf}}</td>
      <td></td>
    </tr>
    </tbody>
  </table>
</template>