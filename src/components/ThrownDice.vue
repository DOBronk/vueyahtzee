<script setup>
// Displays the thrown dices and contains the logic for a new throw
import Dice from "./Dice.vue";
const mod = defineModel();
function rollDice() {
  let tempArray = [];

  // Roll the dice 5 times
  for (let i = 0; i < 5; i++) {
    let val = Math.floor(Math.random() * 6 + 1);
    tempArray.push(val);
  }

  mod.value = tempArray;
}
</script>

<template>
<table>
  <tr>
    <!-- Thrown dices -->
    <td v-for="(dice, index) in mod" :key="index">
      <Dice :number="dice" />
    </td>
  </tr>
  <tr>
    <td colspan="5">
      <div style="text-align: center;">
        <input type="button" value="Rollen" @click="rollDice" />
      </div>
    </td>
  </tr>
</table>
</template>
