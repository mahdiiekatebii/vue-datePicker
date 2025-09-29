<script setup>
import { ref, watch, defineModel } from "vue";
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap";
import modal from "bootstrap/js/src/modal.js";
import DateObject from "react-date-object";
import persian from "react-date-object/calendars/persian";
import persian_fa from "react-date-object/locales/persian_fa";
import persian_en from "react-date-object/locales/persian_en";
import gregorian_fa from "react-date-object/locales/gregorian_fa";
import gregorian_en from "react-date-object/locales/gregorian_en";

let date = new DateObject();
let weekDays = ref([]);
let lenMon = ref([]);
let current_year = ref();
let current_month = ref();
let current_day = ref();
let firstDayOfMonth = ref();
let firstSelect = ref();
let lastSelect = ref();
let monList = ref([]);
let selectMonth = ref("");
let monthName = ref("");
let current_date = ref({
  year: "",
  month: "",
  day: "",
});
let inputYear = ref("");
const props = defineProps(["type", "lan"]);

let model = defineModel();

function createCalendar() {
  const days = document.querySelectorAll("#days");
  for (let i = 0; i < days.length; i++) {
    days[i].classList.remove("selected");
    days[i].classList.remove("firstDay");
    days[i].classList.remove("lastDay");

    if (days[i].classList.contains("currentDay")) {
      days[i].style = "";
    }
  }

  if (props.type === "persian") {
    if (
        selectMonth.value != "" ||
        current_year.value !== current_date.value.year
    ) {
      console.log(selectMonth.value, "month");

      date = new DateObject({
        date: new Date(),
        calendar: persian,
        locale: props.lan === "fa" ? persian_fa : persian_en,
        year: current_year.value,
        month: selectMonth.value,
        day: 1,
      });
    } else {
      date = new DateObject({
        date: new Date(),
        calendar: persian,
        locale: props.lan === "fa" ? persian_fa : persian_en,
      });
    }
    console.log(date);

    current_day.value =
        date.day === current_date.value.day &&
        current_date.value.month === date.month.number
            ? current_date.value.day
            : date.day !== current_date.value.day &&
            current_date.value.month === date.month.number
                ? current_date.value.day
                : "";
    current_month.value = date.month.number;
    current_year.value = date.year;
    weekDays.value = date.weekDays;
    lenMon.value = date.month.length;
    monList.value = date.months;
    selectMonth.value = date.month.number;
    monthName.value = date.month.name;

    let { weekDay } = date.toFirstOfMonth();
    firstDayOfMonth.value = weekDay.index;
  } else {
    if (
        selectMonth.value != "" ||
        current_year.value !== current_date.value.year
    ) {
      date = new DateObject({
        date: new Date(),
        locale: props.lan === "fa" ? gregorian_fa : gregorian_en,
        year: current_year.value,
        month: selectMonth.value,
        day: 1,
      });
    } else {
      console.log("else");

      date = new DateObject({
        date: new Date(),
        locale: props.lan === "fa" ? gregorian_fa : gregorian_en,
      });
    }

    current_day.value =
        date.day === current_date.value.day &&
        current_date.value.month === date.month.number
            ? current_date.value.day
            : date.day !== current_date.value.day &&
            current_date.value.month === date.month.number
                ? current_date.value.day
                : "";
    current_month.value = date.month.number;
    current_year.value = date.year;
    weekDays.value = date.weekDays;
    lenMon.value = date.month.length;
    monList.value = date.months;
    selectMonth.value = date.month.number;
    monthName.value = date.month.name;

    let { weekDay } = date.toFirstOfMonth();
    firstDayOfMonth.value = weekDay.index;
  }
}

watch(
    () => [props.type, props.lan],
    () => {
      if (props.type === "persian") {
        date = new DateObject({
          date: new Date(),
          calendar: persian,
          locale: props.lan === "fa" ? persian_fa : persian_en,
        });

        current_date.value = {
          year: date.year,
          month: date.month.number,
          day: date.day,
        };
        current_year.value = date.year;
        current_month.value = date.month.number;
        selectMonth.value = date.month.number;
        monthName.value = date.month.name;
      } else {
        date = new DateObject({
          date: new Date(),
          locale: props.lan === "fa" ? gregorian_fa : gregorian_en,
        });
        current_date.value = {
          year: date.year,
          month: date.month.number,
          day: date.day,
        };

        current_year.value = date.year;
        current_month.value = date.month.number;
        selectMonth.value = date.month.number;
        monthName.value = date.month.name;
      }
      createCalendar();
    },
    { immediate: true }
);

function click_day(e) {
  model.value = "";
  const days = document.querySelectorAll("#days");
  if (!firstSelect.value) {
    for (let i = 0; i < days.length; i++) {
      days[i].classList.remove("selected");
      days[i].classList.remove("firstDay");
      days[i].classList.remove("lastDay");
      if (days[i].classList.contains("currentDay")) {
        days[i].style.borderRadius = "9px";
        days[i].style.color = "white";
        days[i].style.background = "#0269b8";
      }
    }
    firstSelect.value = e;
    for (let i = 0; i < days.length; i++) {
      if (Number(days[i].getAttribute("day")) === e) {
        days[i].classList.add("firstDay");
      }
    }
  } else {
    lastSelect.value = e;
    for (let i = 0; i < days.length; i++) {
      if (Number(days[i].getAttribute("day")) === e) {
        days[i].classList.add("lastDay");
      }
    }
  }

  if (firstSelect.value && lastSelect.value) {
    range_select();
  }
}

function range_select() {
  const days = document.querySelectorAll("#days");
  for (let i = 0; i < days.length; i++) {
    if (
        days[i].getAttribute("day") >= firstSelect.value &&
        days[i].getAttribute("day") <= lastSelect.value
    ) {
      days[i].classList.add("selected");
      if (days[i].classList.contains("currentDay")) {
        days[i].style.borderRadius = 0;
        days[i].style.color = "black";
        days[i].style.background = "#c8c8c84a";
      }
    }
  }

  model.value =
      current_year.value +
      "/" +
      current_month.value +
      "/" +
      firstSelect.value +
      "to" +
      current_year.value +
      "/" +
      current_month.value +
      "/" +
      lastSelect.value;
  firstSelect.value = null;
  lastSelect.value = null;
}

function selectedMonth(event) {
  console.log("update:modelValue", event.target.value);
  selectMonth.value = event.target.value;
  createCalendar();
}

function go_year() {
  if (inputYear.value) {
    current_year.value = Number(inputYear.value);
    createCalendar();
  }
}

function handleYear(event) {
  inputYear.value = event.target.value;
}
</script>

<template>
  <div class="d-flex justify-content-around">
    <div class="d-flex gap-2 my-4">
      <div class="dropdown">
        <button
            class="btn btn-secondary dropdown-toggle"
            type="button"
            data-bs-toggle="dropdown"
            aria-expanded="false"
        >
          {{ monthName }}
        </button>
        <ul class="dropdown-menu">
          <li v-for="mon in monList">
            <div class="form-check text-start">
              <input
                  :value="mon.number"
                  class="form-check-input"
                  type="radio"
                  name="radioDefault"
                  id="radioDefault"
                  checked
                  @input="selectedMonth"
              />
              <label class="form-check-label" for="radioDefault">
                {{ mon.name }}
              </label>
            </div>
          </li>
        </ul>
      </div>
      <input v-model="model" readonly />
      <div class="dropdown">
        <button
            class="btn btn-secondary dropdown-toggle"
            type="button"
            data-bs-toggle="dropdown"
            aria-expanded="false"
        >
          {{ current_year }}
        </button>
        <ul class="dropdown-menu">
          <li class="enter_year">
            <input :value="inputYear" @input="handleYear" />
            <button type="button" class="btn_year" @click="go_year()">
              برو به سال
            </button>
          </li>
        </ul>
      </div>
    </div>
  </div>
  <div class="container text-center" style="width: 23rem">
    <div class="day_grid">
      <div class="col" v-for="w in weekDays">
        {{ w.shortName }}
      </div>
    </div>
    <div class="day_grid">
      <div v-for="day in firstDayOfMonth"></div>
      <div
          id="days"
          :day="d"
          v-for="d in lenMon"
          key="d"
          @mousedown="click_day(d)"
          :class="[
          d === current_day ? 'currentDay' : 'custom',
          (firstDayOfMonth + d) % 7 === 0 ? 'friday' : '',
        ]"
      >
        {{ d }}
      </div>
    </div>
  </div>
</template>

<style>
.day_grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-auto-rows: 1fr;
  width: 100%;
  row-gap: 5px;
}

.custom {
  cursor: pointer;
}

.currentDay {
  background: #0269b8;
  color: white;
  border-radius: 9px;
}

.selected {
  background-color: #c8c8c84a;
}

.firstDay {
  background: #000000;
  color: white;
  border-bottom-right-radius: 10px;
  border-top-right-radius: 10px;
}

.lastDay {
  background: #000000;
  color: white;
  border-bottom-left-radius: 10px;
  border-top-left-radius: 10px;
}

.friday {
  color: #ff0000;
}
.enter_year {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
  padding: 0.2rem;
}
.btn_year {
  height: 30px;
  width: max-content;
}
#days {
  width: 100%;
  height: 3rem;
  text-align: center;
  align-items: center;
  display: flex;
  justify-content: center;
}
</style>
