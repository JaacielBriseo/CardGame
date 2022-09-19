<script setup>
import { ref, h } from "vue";
import btnReset from "../composables/resetbtn";
import "../assets/index.css";
import "../assets/styles.css";
import Loser from "./Loser.vue";
import Winner from "./Winner.vue";
import "../assets/backg.css";

const perdiste = ref("false");
const winner = ref("false");
const btnAppear = ref(true);
const startButton = ref("Start Game");
const card = ref("");
const aces = ref(4);
const values = [
  "Ace",
  "2",
  "3",
  "4",
  "5",
  "6",
  "7",
  "8",
  "9",
  "10",
  "J",
  "Q",
  "K",
];
const suits = ["♥", "♦", "♠", "♣"];
const cards = [];
const cardsLeft = ref(cards.length);
let cardValues = [];
//Start the game
const deckConstructor = values.forEach(element =>{
  element + suits
})
console.log(deckConstructor)
const deckBuilder = () => {
  for (let s = 0; s < suits.length; s++) {
    for (let v = 0; v < values.length; v++) {
      const value = values[v];
      const suit = suits[s];
      cards.push({ value, suit });
      cardsLeft.value = cards.length;
    }
  }
  startButton.value = "";
};
// Get 5 random cards from the deck
const dealCards = () => {
  let selectedCards = [];

  if (cards.length > 2) {
    for (let index = 0; index < 5; index++) {
      let randomCards = Math.floor(Math.random() * cards.length);
      let randomValue = cards[randomCards];
      selectedCards.push(randomValue);
      cards.splice(randomCards, 1);
      cardsLeft.value = cards.length;
    }
  } else if (cards.length <= 2) {
    for (let index = 0; index < 2; index++) {
      let randomCards = Math.floor(Math.random() * cards.length);
      let randomValue = cards[randomCards];
      selectedCards.push(randomValue);
      cards.splice(randomCards, 1);
      cardsLeft.value = cards.length;
    }
  }
  let fullhand = selectedCards.map((element) => {
    return element.value + element.suit;
  });
  fullhand.forEach((element) => cardValues.push(element));
  if(cardValues.length > 5){
    cardValues.splice(0,5)
  }
  const detectAce = selectedCards.filter((element) => element.value === "Ace");
  if(detectAce.length != 0){aces.value = aces.value - detectAce.length;}
  if(aces.value === 0 && cardsLeft.value >= 2){return (perdiste.value = "");}
  if(aces.value === 0 && cardsLeft.value === 0){return (winner.value = "");}

};
</script>

<template>
  <div class="grid place-items-center" id="container" v-if="startButton">
    <button
      class="m-5 p-2.5 rounded-3xl bg-yellow-400 text-black text-4xl w-64 h-16"
      id="btnStart"
      @click="deckBuilder()"
    >
      {{ startButton }}
    </button>
  </div>
  <div class="grid place-items-center" v-if="!startButton" id="container">
    <Winner v-if="!winner" />
    <div
      class="m-5 text-2xl text-center bg-black text-white w-56 h-auto border divide-solid font-lora"
      id="blackbox"
    >
      <h3>Cards left:</h3>

      <h4>{{ cardsLeft }}</h4>
      <p>Aces left:{{ aces }}</p>
    </div>

    <div class="flex content-evenly" id="cards-container">
      <div
        class="bg-gray-200 text-4xl text-center rounded-3xl m-2.5 w-28 h-44 text-red-600"
        v-for="(element, index) in cardValues"
        :key="index"
      >
        {{ element }} <br />
      </div>
    </div>
    <Loser v-if="!perdiste" />

    <button
      class="m-5 rounded-3xl p-2.5 bg-yellow-400 w-64 h-20 text-4xl"
      id="btnDeal"
      v-if="perdiste && winner"
      @click="dealCards()"
    >
      DEAL
    </button>
    <br />
    <button
      class="text-xl rounded-3xl text-yellow-400 p-2.5 m-2.5 w-36 border-solid border-yellow-400 border-2 bg-transparent"
      id="btnReset"
      v-if="perdiste && winner"
      @click="btnReset()"
    >
      Reset
    </button>
    <button
      class="text-xl rounded-3xl text-yellow-400 p-2.5 m-2.5 w-36 border-solid border-yellow-400 border-2 bg-transparent"
      id="btnReset"
      v-if="!perdiste || !winner"
      @click="btnReset()"
    >
      Play again
    </button>
  </div>
</template>
