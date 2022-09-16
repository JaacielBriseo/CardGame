<script setup>
import { ref } from "vue";
import btnReset from "../composables/resetbtn";
import "../assets/styles.css";
import Loser from "./Loser.vue";
import Winner from "./Winner.vue";
import "../assets/backg.css"

const perdiste = ref("false");
const winner = ref("false");
const btnAppear = ref(true);
const startButton = ref("Start Game");
const carta1 = ref("");
const carta2 = ref("");
const carta3 = ref("");
const carta4 = ref("");
const carta5 = ref("");
const suit1 = ref("")
const suit2 = ref("")
const suit3 = ref("")
const suit4 = ref("")
const suit5 = ref("")
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
  // console.log(cards);
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


  //console.log(cards);

  const detectAce = selectedCards.filter((element) => element.value === "Ace");

  const cardValues = selectedCards.map((element) => {
    return element.value + element.suit;
  });

console.log(cardValues)
if(cardValues.length === 5){
suit1.value=selectedCards[0].suit
suit2.value=selectedCards[1].suit
suit3.value=selectedCards[2].suit
suit4.value=selectedCards[3].suit
suit5.value=selectedCards[4].suit}
else if(cardValues.length <= 2){
suit1.value=selectedCards[0].suit
suit2.value=selectedCards[1].suit
}
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
    <button id="btnStart" @click="deckBuilder()">{{ startButton }}</button>
  </div>
  <div v-if="!startButton" id="container">
          <Winner v-if="!winner" />
    <div id="blackbox">
    
      <h3>Cards left:</h3>

      <h4>{{ cardsLeft }}</h4>
      <p>Aces left:{{ aces }}</p>
    </div>

      <div id="cards-container">
        <div >{{ carta1 }} <br> <a id="suitText"> {{suit1}} </a></div>
        <div >{{ carta2 }} <br><sub id="suitText"> {{suit2}} </sub></div>
        <div  v-if="cardsLeft >= 2">{{ carta3 }} <br> <sub id="suitText"> {{suit3}} </sub></div>
        <div  v-if="cardsLeft >= 2">{{ carta4 }} <br> <sub id="suitText"> {{suit4}} </sub></div>
        <div  v-if="cardsLeft >= 2">{{ carta5 }} <br> <sub id="suitText"> {{suit5}} </sub></div>
      </div>
      <Loser v-if="!perdiste" />

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
