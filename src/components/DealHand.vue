<script setup>
import { ref } from "vue";
import btnReset from "../composables/resetbtn";
import "../assets/index.css";
import Loser from "./Loser.vue";
import Winner from "./Winner.vue";


const perdiste = ref("false");
const winner = ref("false");
const btnAppear = ref(true);
const startButton = ref("Start Game");
const carta1 = ref("");
const carta2 = ref("");
const carta3 = ref("");
const carta4 = ref("");
const carta5 = ref("");
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

//Start the game

const deckBuilder = () => {
    for (let s = 0; s < suits.length; s++) {
      for (let v = 0; v < values.length; v++) {
        const value = values[v];
        const suit = suits[s];
        cards.push({ value, suit });
        cardsLeft.value = cards.length;
      }
    }
    console.log(cards);
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
      console.log(cards.length);
    }
  } else if (cards.length <= 2) {
    for (let index = 0; index < 2; index++) {
      let randomCards = Math.floor(Math.random() * cards.length);
      let randomValue = cards[randomCards];
      selectedCards.push(randomValue);
      cards.splice(randomCards, 1);
      cardsLeft.value = cards.length;
      console.log(cards.length);
    }
  }

  console.log(selectedCards);
  console.log(cards);

  const detectAce = selectedCards.filter((element) => element.value === "Ace");

  const cardValues = selectedCards.map((element) => {
    return element.value + element.suit;
  });

  carta1.value = cardValues[0];
  carta2.value = cardValues[1];
  carta3.value = cardValues[2];
  carta4.value = cardValues[3];
  carta5.value = cardValues[4];

  if (detectAce.length != 0) {
    aces.value = aces.value - detectAce.length;
  }
  //console.log(detectAce);
  if (aces.value === 0 && cardsLeft.value >= 2) {
    return (perdiste.value = "");
  }
  if (aces.value === 0 && cardsLeft.value === 0) {
    return (winner.value = "");
  }
};
</script> 

<template>
  <div id="container" v-if="startButton">
    <button id="btnDeal" @click="deckBuilder()">{{ startButton }}</button>
  </div>

  <div v-if="!startButton" id="container">
    <Winner v-if="!winner" />
    <div id="blackbox">
      <h3>Cards left:</h3>

      <h4>{{ cardsLeft }}</h4>
      <p>Aces left:{{ aces }}</p>
    </div>

    <div id="containerCartas">
      <div id="cards-container">
        <div>{{ carta1 }}</div>
        <div>{{ carta2 }}</div>
        <div>{{ carta3 }}</div>
        <div>{{ carta4 }}</div>
        <div>{{ carta5 }}</div>
      </div>
      <Loser v-if="!perdiste" />
    </div>

    <button id="btnDeal" v-if="perdiste && winner" @click="dealCards()">
      DEAL
    </button>
    <br />
    <button id="btnReset" v-if="perdiste && winner" @click="btnReset()">
      Reset
    </button>

    <button id="btnReset" v-if="!perdiste || !winner" @click="btnReset()">
      Play again
    </button>
  </div>
</template>

<style>
#container {
  display: grid;
  place-items: center;
}
#blackbox {
  margin: 20px;
  font-size: 25px;
  text-align: center;
  background-color: black;
  color: white;
  width: 230px;
  height: 2;
  border-style: solid;
  border-color: yellow;
  border-width: 1px;
}
#btnDeal {
  margin: 20px;
  border-radius: 25px;
  padding: 20px;
  background-color: yellow;
  color: black;
  font-size: 40px;
  width: 300px;
}
#btnReset {
  font-size: 20px;
  border-radius: 25px;
  background-color: transparent;
  color: yellow;
  padding: 10px;
  margin: 10px;
  width: 150px;
  border-style: solid;
  border-color: yellow;
  border-width: 1px;
}
#cards-container {
  display: flex;
  align-content: space-between;
  grid-template-columns: 100px 100px 100px;
  gap: 45px;
  padding: 45px;
}
#cards-container > div {
  background-color: rgba(255, 255, 255, 0.8);
  text-align: center;
  padding: 20px 0;
  font-size: 50px;
  border-radius: 15px;
  margin: 10px;
  padding: 10px;
  width: 125px;
  height: 200px;
}
</style>


