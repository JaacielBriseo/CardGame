<script setup>
import btnReset from "../composables/resetbtn";
import "../assets/fonts.css";
</script>
<template>
  <div>
    <h1 id="looserText">You lose.</h1>
    <h1 id="looserText">Better luck next time!</h1>
  </div>
</template>

<style>

#looserText {
  color: white;
  font-size: 30px;
  text-align: center;
  font-family: 'Spline Sans Mono', monospace;
}
</style>