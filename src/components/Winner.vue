<script setup>
import DealHand from './DealHand.vue'
import ConfettiExplosion from "vue-confetti-explosion";
</script>

<template>
  <div class="ribbon">
    <div class="ribbon-stitches-top"></div>
    <div class="ribbon-content">
        <h1><b>WINNER !</b></h1>
    </div>
    <div class="ribbon-stitches-bottom"></div>
     
</div>
<ConfettiExplosion />
</template>

<style >
.ribbon {
     width: 560px;
     position: absolute;
    margin-top: -325px;
     text-align: center;
     font-size: 25px !important;
     background: #d47429;
     background: -webkit-gradient(linear, left top, left bottom, from(#d47429), to(#ec761d));
     background: -webkit-linear-gradient(top, #d47429, #ec761d);
     background: -moz-linear-gradient(top, #d47429, #ec761d);
     background: -ms-linear-gradient(top, #d47429, #ec761d);
     background: -o-linear-gradient(top, #d47429, #ec761d);
     background-image: -ms-linear-gradient(top, #d47429 0%, #ec761d 100%);
     -webkit-box-shadow: rgba(0,0,0,0.3) 0 0 0;
     -moz-box-shadow: rgba(0,0,0,0.3) 0 0 0;
     box-shadow: rgba(0,0,0,0.3) 0 0 0;
     font-family: 'Helvetica Neue',Helvetica, sans-serif;
}

.ribbon h1 {
     font-size: 39px !important;
     color: #dbe4d3;
     text-shadow: #020202 0 1px 0;
     margin:0px;
     padding: 10px 10px;
}

.ribbon:before, .ribbon:after {
     content: '';
     position: absolute;
     display: block;
     bottom: -1em;
     border: 1.5em solid #e3863e;
     z-index: -1;
}

.ribbon:before {
     left: -2em;
     border-right-width: 1.5em;
     border-left-color: transparent;
}

.ribbon:after {
     right: -2em;
     border-left-width: 1.5em;
     border-right-color: transparent;
}

.ribbon .ribbon-content:before, .ribbon .ribbon-content:after {
     border-color: #bc671d transparent transparent transparent;
     position: absolute;
     display: block;
     border-style: solid;
     bottom: -1em;
     content: '';
}

.ribbon .ribbon-content:before {
     left: 0;
     border-width: 1em 0 0 1em;
}

.ribbon .ribbon-content:after {
     right: 0;
     border-width: 1em 1em 0 0;
}
</style>