<script setup>
import { inject, provide, reactive, ref, watch } from 'vue';
defineProps(['dataCards']);
</script>

<template>
    <div class="card-back">
        <img id="cardBack" src="/images/bg-card-back.png" alt="">
        <span id="backNumber">{{ dataCards.cvv === '' ? '000' : dataCards.cvv }}</span>
    </div>

    <div class="card-front">
        <img id="cardFront" src="/images/bg-card-front.png" alt="">
        <img src="/images/card-logo.svg" alt="" id="iconFront">
        <span id="numberCard">{{ dataCards.numberCard === '' ? '0000 0000 0000 0000' : dataCards.numberCard }}</span>
        <div id="bottomCard">
            <span id="nameCard">{{ dataCards.userCard === '' ? 'Name' : dataCards.userCard }}</span>
            <span id="expiryDate">{{ dataCards.expMonth === '' ? '00' : dataCards.expMonth }}/{{ dataCards.expYear === '' ? '00' : dataCards.expYear }}</span>
        </div>
    </div>
</template>