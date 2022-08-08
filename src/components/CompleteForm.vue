<script setup>
const reloadPage = () => window.location.reload();
</script>

<template>
    <div class="flex" style="flex-direction: column; gap: 1rem;">
        <img src="images/icon-complete.svg" alt="icon complete">
        <h1>Thank you!</h1>
        <p>We've added your card details</p>
        <button @click="reloadPage">Continue</button>
    </div>
</template>