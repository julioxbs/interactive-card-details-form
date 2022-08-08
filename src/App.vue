<script setup>
import { computed, provide, reactive, ref, watch } from 'vue';
import Form from './components/Form.vue';
import CompleteForm from './components/CompleteForm.vue';
import CardSide from './components/CardSide.vue';

const isValid = ref(false);

const formIsValid = (event) => {
  isValid.value = event;
}

const cardsInputs = reactive({
  numberCard: '',
  userCard: '',
  expMonth: '',
  expYear: '',
  cvv: '',
});

provide('dataCards', computed(() => cardsInputs));

</script>

<template>
  <main>
    <div class="left-side">
      <CardSide :dataCards="cardsInputs" />
    </div>
    <div class="form-side">
      <Form v-if="!isValid" @form-valid="formIsValid" />
      <CompleteForm v-else="isValid" />
    </div>
  </main>
</template>