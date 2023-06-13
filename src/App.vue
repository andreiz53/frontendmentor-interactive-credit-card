<script setup lang="ts">
import IconComplete from './components/IconComplete.vue';
import { ref, computed } from 'vue';

const cardName = ref({
  text: '',
  isValid: true,
});
const cardNumber = ref({
  text: '',
  isValid: true,
  hasLength: true,
});
const cardMonth = ref({
  text: '',
  isValid: true,
  isMonth: true,
});
const cardYear = ref({
  text: '',
  isValid: true,
  isYear: true,
});
const cardCode = ref({
  text: '',
  isValid: true,
  isCode: true,
});
const isConfirmed = ref(false);

const formattedNumber = computed(() => {
  return (
    cardNumber.value.text.slice(0, 4) +
    ' ' +
    cardNumber.value.text.slice(4, 8) +
    ' ' +
    cardNumber.value.text.slice(8, 12) +
    ' ' +
    cardNumber.value.text.slice(12)
  );
});

const validateName = () => {
  if (!cardName.value.text.length) {
    cardName.value.isValid = false;
  } else {
    cardName.value.isValid = true;
  }
};
const validateNumber = () => {
  if (!cardNumber.value.text.length) {
    cardNumber.value.isValid = false;
  } else {
    cardNumber.value.isValid = true;
  }
  if (cardNumber.value.text.length < 16) {
    cardNumber.value.hasLength = false;
  } else {
    cardNumber.value.hasLength = true;
  }
};
const validateMonth = () => {
  if (!cardMonth.value.text.length) {
    cardMonth.value.isValid = false;
  } else {
    cardMonth.value.isValid = true;
  }
  if (!Number(cardMonth.value.text)) {
    cardMonth.value.isMonth = false;
  } else if (cardMonth.value.text === '0' || cardMonth.value.text === '00') {
    cardMonth.value.isMonth = false;
  } else if (Number(cardMonth.value.text) > 12) {
    cardMonth.value.isMonth = false;
  } else {
    cardMonth.value.isMonth = true;
  }
};
const validateYear = () => {
  if (!cardYear.value.text.length) {
    cardYear.value.isValid = false;
  } else {
    cardYear.value.isValid = true;
  }
  let now = new Date();
  let thisYear = String(now.getFullYear()).slice(2);
  if (!Number(cardYear.value.text)) {
    cardYear.value.isYear = false;
  } else if (Number(thisYear) > Number(cardYear.value.text)) {
    cardYear.value.isYear = false;
  } else {
    cardYear.value.isYear = true;
  }
};
const validateCode = () => {
  if (!cardCode.value.text.length) {
    cardCode.value.isValid = false;
  } else {
    cardCode.value.isValid = true;
  }

  if (cardCode.value.text.length < 3) {
    cardCode.value.isCode = false;
  } else {
    cardCode.value.isCode = true;
  }
};

const validatedForm = computed(() => {
  return (
    cardName.value.isValid &&
    cardNumber.value.isValid &&
    cardNumber.value.hasLength &&
    cardMonth.value.isMonth &&
    cardMonth.value.isValid &&
    cardYear.value.isValid &&
    cardYear.value.isYear &&
    cardCode.value.isCode &&
    cardCode.value.isValid &&
    cardCode.value.text
  );
});
const handleSubmit = () => {
  if (validatedForm.value) {
    isConfirmed.value = true;
  } else {
    isConfirmed.value = false;
  }
};
</script>

<template>
  <div id="page-container">
    <section class="main-section">
      <div class="main-row">
        <div class="left-col">
          <div class="card front">
            <div class="circles">
              <div class="circle-big"></div>
              <div class="circle-small"></div>
            </div>
            <div class="card-number" v-if="formattedNumber.trim()">
              {{ formattedNumber }}
            </div>
            <div class="card-number" v-else>0000 0000 0000 0000</div>
            <div class="card-info">
              <div class="card-name" v-if="cardName.text">
                {{ cardName.text }}
              </div>
              <div class="card-name" v-else>JANE APPLESEED</div>
              <div class="card-date">
                <span class="card-month" v-if="cardMonth.text">{{
                  cardMonth.text
                }}</span>
                <span class="card-month" v-else>00</span>
                <span>/</span>
                <span class="card-year" v-if="cardYear.text">{{
                  cardYear.text
                }}</span>
                <span class="card-year" v-else>00</span>
              </div>
            </div>
          </div>
          <div class="card back">
            <div class="card-code" v-if="cardCode.text">
              {{ cardCode.text }}
            </div>
            <div class="card-code" v-else>000</div>
          </div>
        </div>
        <div class="right-col">
          <form v-if="!isConfirmed" @submit.prevent="handleSubmit">
            <div class="form-row">
              <label for="name">CARDHOLDER NAME</label>
              <span class="input-wrapper" :class="{ error: !cardName.isValid }">
                <input
                  :class="{ error: !cardName.isValid }"
                  v-model="cardName.text"
                  type="text"
                  name="name"
                  id="name"
                  placeholder="e.g. Jane Appleseed"
                  @input="validateName"
                />
              </span>
              <p v-if="!cardName.isValid" class="input-error">Can't be blank</p>
            </div>
            <div class="form-row">
              <label for="number">CARD NUMBER</label>
              <span
                class="input-wrapper"
                :class="{ error: !cardNumber.isValid }"
              >
                <input
                  v-model="cardNumber.text"
                  type="text"
                  name="number"
                  id="number"
                  placeholder="e.g. 1234 5678 9123 0000"
                  maxlength="16"
                  @input="validateNumber"
                  :class="{
                    error: !cardNumber.isValid || !cardNumber.hasLength,
                  }"
                />
              </span>
              <p v-if="!cardNumber.isValid" class="input-error">
                Can't be blank
              </p>
              <p v-if="!cardNumber.hasLength" class="input-error">
                Must have 16 digits
              </p>
            </div>
            <div class="form-row split-two">
              <div class="left">
                <label for="month">Exp. Date (MM/YY)</label>
                <div class="inputs-container">
                  <span
                    class="input-wrapper"
                    :class="{ error: !cardMonth.isValid }"
                  >
                    <input
                      v-model="cardMonth.text"
                      type="text"
                      name="month"
                      id="month"
                      placeholder="MM"
                      maxlength="2"
                      @input="validateMonth"
                      :class="{ error: !cardMonth.isValid }"
                    />
                  </span>
                  <span
                    class="input-wrapper"
                    :class="{ error: !cardYear.isValid }"
                    ><input
                      v-model="cardYear.text"
                      type="text"
                      name="year"
                      id="year"
                      placeholder="YY"
                      maxlength="2"
                      @input="validateYear"
                      :class="{ error: !cardYear.isValid }"
                  /></span>
                </div>
                <p v-if="!cardMonth.isValid" class="input-error">
                  Can't be blank
                </p>
                <p v-if="!cardMonth.isMonth" class="input-error">Not a month</p>
                <p v-if="!cardYear.isValid" class="input-error">
                  Can't be blank
                </p>
                <p v-if="!cardYear.isYear" class="input-error">
                  Incorrect or expired
                </p>
              </div>
              <div class="right">
                <label for="cvc">CVC</label>
                <span
                  class="input-wrapper"
                  :class="{ error: !cardCode.isValid }"
                >
                  <input
                    v-model="cardCode.text"
                    type="text"
                    name="cvc"
                    id="cvc"
                    placeholder="e.g. 123"
                    maxlength="3"
                    @input="validateCode"
                    :class="{ error: !cardCode.isValid }"
                  />
                </span>
                <p v-if="!cardCode.isValid" class="input-error">
                  Can't be blank
                </p>
                <p v-if="!cardCode.isCode" class="input-error">
                  Must have 3 digits
                </p>
              </div>
            </div>
            <div class="form-actions">
              <button type="submit" id="submit" :disabled="!validatedForm">
                Confirm
              </button>
            </div>
          </form>
          <div class="thank-you-wrapper" v-else>
            <IconComplete />
            <div class="thank-you-title">THANK YOU!</div>
            <div class="thank-you-desc">We’ve added your card details</div>
            <button class="thank-you-button">Continue</button>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
