<script setup>
import { computed, reactive, ref, watch, provide, inject } from "vue";

const cardsData = inject("dataCards");
const emit = defineEmits(["formValid"]);

const state = reactive({
  name: {
    value: "",
    error: false,
    message: "",
  },
  cardNumber: {
    value: "",
    error: false,
    message: "",
  },
  expMonth: {
    value: "",
    error: false,
    message: "",
  },
  expYear: {
    value: "",
    error: false,
    message: "",
  },
  cvc: {
    value: "",
    error: false,
    message: "",
  },
});

const validationInputs = () => {
  const { name, cardNumber, expMonth, expYear, cvc } = state;

  // Name Input
  let nameContainsNumber = name.value.match(/\d/);
  if (name.value === "") {
    name.error = true;
    name.message = "Can't be blank";
  } else if (nameContainsNumber) {
    name.error = true;
    name.message = "Can't contain numbers";
  } else {
    name.error = false;
    name.message = "";
  }

  // Card Number Input
  let cardNumberContainsLetter = cardNumber.value.match(/[a-z]/i);
  let totalNumbers = cardNumber.value.replaceAll(" ", "").length;
  if (cardNumber.value === "") {
    cardNumber.error = true;
    cardNumber.message = "Can't be blank";
  } else if (cardNumberContainsLetter) {
    cardNumber.error = true;
    cardNumber.message = "Can't contain letters";
  } else if (totalNumbers > 16 || totalNumbers < 16) {
    cardNumber.error = true;
    cardNumber.message = "Must be 16 digits";
  } else {
    cardNumber.error = false;
    cardNumber.message = "";
  }

  // Exp Month Input
  if (expMonth.value === "") {
    expMonth.error = true;
    expMonth.message = "Can't be blank";
  } else if (expMonth.value <= 0) {
    expMonth.error = true;
    expMonth.message = "Must be valid month";
  } else {
    expMonth.error = false;
    expMonth.message = "";
  }

  // Exp Year Input
  if (expYear.value === "") {
    expYear.error = true;
    expYear.message = "Can't be blank";
  } else if (expYear.value <= 0 || String(expYear.value).split("").length > 2 || String(expYear.value).split("").length < 2) {
    expYear.error = true;
    expYear.message = "Must be valid year";
  } else {
    expYear.error = false;
    expYear.message = "";
  }

  // CVC Input
  if (cvc.value === "") {
    cvc.error = true;
    cvc.message = "Can't be blank";
  } else if (
    String(cvc.value).split("").length < 3 ||
    String(cvc.value).split("").length > 3
  ) {
    cvc.error = true;
    cvc.message = "Must be 3 positive digits";
  } else {
    cvc.error = false;
    cvc.message = "";
  }

  if (
    name.error ||
    cardNumber.error ||
    expMonth.error ||
    expYear.error ||
    cvc.error
  ) {
    return false;
  } else {
    return true;
  }
};

const submitValues = () => {
  const resultOfValidation = validationInputs();
  emit("formValid", resultOfValidation);
};
</script>

<template>
  <form @submit.prevent="submitValues">
    <label for="cardName">
      <span class="inputsNames">CARDHOLDER NAME</span>
      <input
        :class="state.name.error && state.name.value === '' ? 'errorInput' : ''"
        id="cardName"
        name="cardName"
        type="text"
        placeholder="e.g. Jane Applessed"
        v-model.trim="state.name.value"
        v-model="cardsData.userCard"
      />

      <!-- Message Error -->
      <p v-show="state.name.error" class="errorMessage">
        {{ state.name.message }}
      </p>
    </label>

    <label for="cardNumber">
      <span class="inputsNames">CARD NUMBER</span>
      <input
        :class="
          state.cardNumber.error && state.cardNumber.value === ''
            ? 'errorInput'
            : ''
        "
        id="cardNumber"
        name="cardNumber"
        type="text"
        placeholder="e.g. 1234 5678 9123 0000"
        v-model.trim="state.cardNumber.value"
        v-model="cardsData.numberCard"
      />

      <!-- Message Error -->
      <p v-show="state.cardNumber.error" class="errorMessage">
        {{ state.cardNumber.message }}
      </p>
    </label>

    <div>
      <span class="inputsNames">EXP. DATE (MM/YY) CVV</span>

      <div id="inputs">
        <div class="mmYY">
          <div class="flex" style="gap: 5px">
            <input
              :class="state.expMonth.error ? 'errorInput' : ''"
              class="dateInput"
              name="expMonth"
              type="number"
              placeholder="MM"
              v-model.trim="state.expMonth.value"
              v-model="cardsData.expMonth"
            />

            <input
              :class="state.expYear.error ? 'errorInput' : ''"
              class="dateInput"
              name="expYear"
              type="number"
              placeholder="YY"
              v-model.trim="state.expYear.value"
              v-model="cardsData.expYear"
            />
          </div>
          <p :class="state.expYear.error ? 'errorMessage' : 'hide'">
            {{ state.expYear.message }}
          </p>
        </div>

        <div class="flex flex-column">
          <input
            :class="state.cvc.error ? 'errorInput' : ''"
            id="cvcInput"
            name="cvc"
            type="number"
            placeholder="e.g. 123"
            v-model.trim="state.cvc.value"
            v-model="cardsData.cvv"
          />

          <!-- Message Error -->
          <p :class="state.cvc.error ? 'errorMessage' : 'hide'">
            {{ state.cvc.message }}
          </p>
        </div>
      </div>
    </div>

    <button type="submit">Confirm</button>
  </form>
</template>
