<script setup>
import { ref } from "vue";
import DateObject from "react-date-object";
import persian from "react-date-object/calendars/persian";
import persian_fa from "react-date-object/locales/persian_fa";
import persian_en from "react-date-object/locales/persian_en";
import gregorian_fa from "react-date-object/locales/gregorian_fa";
import gregorian_en from "react-date-object/locales/gregorian_en";
import "bootstrap/dist/css/bootstrap.min.css";
import Calendar from "./components/Calendar.vue";

let type = ref("persian");
let language = ref("fa");
const date = ref("");

function handleInput(event) {
  type.value = event.target.value;
}

function handleInputLan(event) {
  language.value = event.target.value;
}
</script>

<template>
  <div class="d-flex gap-2">
    <div class="dropdown">
      <button
          class="btn btn-secondary dropdown-toggle"
          type="button"
          data-bs-toggle="dropdown"
          aria-expanded="false"
      >
        نوع تقویم
      </button>
      <ul class="dropdown-menu">
        <li>
          <div class="form-check text-start">
            <input
                value="gregorian"
                class="form-check-input"
                type="radio"
                name="radioType"
                id="radioGregorian"
                @input="handleInput"
            />
            <label class="form-check-label" for="radioGregorian">
              میلادی
            </label>
          </div>
        </li>
        <li>
          <div class="form-check text-start">
            <input
                value="persian"
                class="form-check-input"
                type="radio"
                name="radioType"
                id="radioPersian"
                checked
                @input="handleInput"
            />
            <label class="form-check-label" for="radioPersian"> شمسی </label>
          </div>
        </li>
      </ul>
    </div>
    <div class="dropdown">
      <button
          class="btn btn-secondary dropdown-toggle"
          type="button"
          data-bs-toggle="dropdown"
          aria-expanded="false"
      >
        زبان
      </button>
      <ul class="dropdown-menu">
        <li>
          <div class="form-check text-start">
            <input
                value="fa"
                class="form-check-input"
                type="radio"
                name="radioLang"
                id="radioFa"
                checked
                @input="handleInputLan"
            />
            <label class="form-check-label" for="radioFa"> fa </label>
          </div>
        </li>
        <li>
          <div class="form-check text-start">
            <input
                value="en"
                class="form-check-input"
                type="radio"
                name="radioLang"
                id="radioEn"
                @input="handleInputLan"
            />
            <label class="form-check-label" for="radioEn"> en </label>
          </div>
        </li>
      </ul>
    </div>
  </div>

  <Calendar :type="type" :lan="language" v-model="date" />
  <div>{{ date }}</div>
</template>

<style></style>
