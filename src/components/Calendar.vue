<script setup>
import {ref, watch, defineModel} from "vue";
import DateObject from "react-date-object";
import persian from "react-date-object/calendars/persian";
import persian_fa from "react-date-object/locales/persian_fa";
import persian_en from "react-date-object/locales/persian_en";
import "bootstrap/dist/css/bootstrap.min.css"
import "bootstrap"
import modal from "bootstrap/js/src/modal.js";


let date = new DateObject();
let weekDays = ref([])
let lenMon = ref([])
let current_year = ref()
let current_month = ref()
let current_day = ref()
let firstDayOfMonth = ref()
let firstSelect = ref()
let lastSelect = ref()
let selected_day = ref()
let monList = ref([])
let labelMonths = ref("")
let current_date = ref({
  year: "",
  month: "",
  day: ""
})


const props = defineProps(['type', 'lan'])


let model = defineModel()



function createCalendar() {
  const day = document.querySelectorAll("#days")
  console.log(props.lan)
  for (let i = 0; i < day.length; i++) {
    day[i].classList.remove("selected");
    day[i].classList.remove("firstDay");
    day[i].classList.remove("lastDay");

  }
  if (props.type === "persian") {
    if (labelMonths.value.length != '' || (current_year.value !==  current_date.value.year)) {
      date = new DateObject({
        date: new Date(),
        calendar: persian,
        locale: props.lan === "fa" ? persian_fa : persian_en,
        year: current_year.value,
        month: labelMonths.value,
        day: 1,
      });
    } else {
      date = new DateObject({
        date: new Date(),
        calendar: persian,
        locale: props.lan === "fa" ? persian_fa : persian_en,
      });
    }


    current_day.value = date.day === current_date.value.day && current_date.value.month === date.month.number ? current_date.value.day :
        date.day !== current_date.value.day && current_date.value.month === date.month.number ? current_date.value.day : ""
    current_month.value = date.month.number
    current_year.value = date.year
    weekDays.value = date.weekDays
    lenMon.value = date.month.length
    monList.value = date.months
    labelMonths.value = date.month.name
    let {year, month, weekDay, day} = date.toFirstOfMonth()
    firstDayOfMonth.value = weekDay.index

  } else {
    date = new DateObject({
      date: new Date(),
      // calendar: persian,
      locale: persian_fa,
      // year: data.year,
      // month: data.month,
      // day: data.day,
    });
    console.log(date, "date")

    current_day.value = date.day === current_date.value.day && current_date.value.month === date.month.number ? current_date.value.day :
        date.day !== current_date.value.day && current_date.value.month === date.month.number ? current_date.value.day : ""
    current_month.value = date.month.number
    current_year.value = date.year
    weekDays.value = date.weekDays
    lenMon.value = date.month.length
    monList.value = date.months

    let {year, month, weekDay, day} = date.toFirstOfMonth()
    firstDayOfMonth.value = weekDay.index
  }

}


watch(() => [props.type, props.lan], () => {
  if (props.type === "persian") {
    date = new DateObject({
      date: new Date(),
      calendar: persian,
      locale: props.lan === "fa" ? persian_fa : persian_en,
    });

    current_date.value =
        {
          year: date.year,
          month: date.month.number,
          day: date.day,

        }
    current_year.value=date.year
  } else {
    current_year.value=date.year
    date = new DateObject({
      date: new Date(),
    });
    current_date.value = [
      {
        year: date.year,
        month: date.month.number,
        day: date.day,

      }
    ]
  }
  createCalendar()
}, {immediate: true});


function click_day(e) {
  model.value = ''
  const day = document.querySelectorAll("#days")
  if (!firstSelect.value) {
    for (let i = 0; i < day.length; i++) {
      day[i].classList.remove("selected");
      day[i].classList.remove("firstDay");
      day[i].classList.remove("lastDay");
      if(day[i].classList.contains("currentDay")) {
        day[i].style.borderRadius="9px";
        day[i].style.color="white";
      }

    }
    firstSelect.value = e
    for (let i = 0; i < day.length; i++) {
      if (Number(day[i].getAttribute("day")) === e) {
        day[i].classList.add("firstDay");
      }
    }

  } else {
    lastSelect.value = e
    for (let i = 0; i < day.length; i++) {
      if (Number(day[i].getAttribute("day")) === e) {
        day[i].classList.add("lastDay");
      }
    }
  }

  if (firstSelect.value && lastSelect.value) {
    range_select(firstSelect.value, lastSelect.value)
  }

}

function range_select(f, l) {
  const day = document.querySelectorAll("#days")
  for (let i = 0; i < day.length; i++) {
    if (day[i].getAttribute("day") >= firstSelect.value && day[i].getAttribute("day") <= lastSelect.value) {
      day[i].classList.add("selected")
     if(day[i].classList.contains("currentDay")) {
       day[i].style.borderRadius=0;
       day[i].style.color="black";
     }
    }
  }

  model.value = current_year.value + "/" + current_month.value + "/" + firstSelect.value + "to" + current_year.value + "/" + current_month.value + "/" + lastSelect.value
  firstSelect.value = null
  lastSelect.value = null
}

function selectMonth(event) {
  console.log('update:modelValue', event.target.value);
  labelMonths.value = event.target.value;
  createCalendar()

}


function go_year(year){
  console.log(year,"vallue")
}

function handleYear(event) {
  current_year.value = event.target.value
}



</script>

<template>
  <div class="d-flex justify-content-around">


<!--    <button>‹</button>-->
    <div class="d-flex gap-2">
      <div class="dropdown">
        <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
          {{ labelMonths }}
        </button>
        <ul class="dropdown-menu">
          <li v-for="mon in monList">
            <div class="form-check text-start">
              <input :value="mon.number" class="form-check-input" type="radio" name="radioDefault" id="radioDefault"
                     checked @input="selectMonth"/>
              <label class="form-check-label" for="radioDefault">
                {{ mon.name }}
              </label>
            </div>
          </li>
        </ul>
      </div>
      <div class="dropdown" >
        <button class="btn btn-secondary dropdown-toggle " type="button" data-bs-toggle="dropdown" aria-expanded="false">
          {{current_year}}
        </button>
        <ul class="dropdown-menu">
          <li class="enter_year">
            <input :value="current_year" @input="handleYear"/>
            <button type="button" class="btn_year" @click="go_year(current_year)">برو به سال</button>
          </li>
        </ul>
      </div>
    </div>
<!--    <button>›</button>-->
  </div>
  <div class="container text-center" style="width: 23rem">
    <input v-model="model" readonly>
    <div class="day_grid">
      <div class="col" v-for="w in weekDays">
        {{ w.name }}
      </div>

    </div>
    <div class="day_grid">
      <div v-for="day in firstDayOfMonth">
      </div>
      <div id="days" :day="d" v-for="d in lenMon" key="d" @mousedown="click_day(d)"
           :class="[d === current_day ? 'currentDay':'custom',(firstDayOfMonth + (d)) % 7 === 0   ? 'friday' :'']">
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
.enter_year{
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
  padding: 0.2rem;
}
.btn_year{
  height: 30px;
  width: max-content;
}
#days{
  width:100%;
  height: 3rem;
  text-align: center;
  align-items: center;
  display: flex;
  justify-content: center;

}
</style>
