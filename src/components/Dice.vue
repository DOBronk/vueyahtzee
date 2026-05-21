<script setup>
// Visual representation of our dice
import Dice1 from "../assets/Dice1.svg";
import Dice2 from "../assets/Dice2.svg";
import Dice3 from "../assets/Dice3.svg";
import Dice4 from "../assets/Dice4.svg";
import Dice5 from "../assets/Dice5.svg";
import Dice6 from "../assets/Dice6.svg";

const props = defineProps({ number: Number});
const imgDice = [Dice1, Dice2, Dice3, Dice4, Dice5, Dice6];
</script>

<template>
 <img alt="A dice" :src="imgDice[props.number - 1]" />
</template>
