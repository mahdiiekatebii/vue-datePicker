<script setup>
import {ref,defineModel} from "vue";
import DateObject from "react-date-object";
import persian from "react-date-object/calendars/persian";
import persian_fa from "react-date-object/locales/persian_fa";
import "bootstrap/dist/css/bootstrap.min.css"
import "bootstrap"
import Calendar from "./components/Calendar.vue";


let date = new DateObject();

let type=ref("persian")
let language=ref("fa")




const msg = ref('')


// dateFormat(type,{ year: 1404,
//   month:1,
//   day: 1,})




    function handleInput(event) {
      type.value = event.target.value;
      // dateFormat(type, {
      //   year:current_year.value,
      //   month:event.target.value,
      //   day: 1,
      // })
    }

function handleInputLan(event) {
  language.value = event.target.value;
  // dateFormat(type, {
  //   year:current_year.value,
  //   month:event.target.value,
  //   day: 1,
  // })
}


</script>

<template>
  <div class="dropdown">
    <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
      نوع تقویم
    </button>
    <ul class="dropdown-menu">
      <li>
        <div class="form-check text-start">
          <input value="gregorian" class="form-check-input" type="radio" name="radioDefault" id="radioDefault"
                 checked  @input="handleInput"/>
          <label class="form-check-label" for="radioDefault">
         میلادی
          </label>
        </div>
      </li>
      <li>
        <div class="form-check text-start">
          <input value="persian" class="form-check-input" type="radio" name="radioDefault" id="radioDefault"
                 checked  @input="handleInput"/>
          <label class="form-check-label" for="radioDefault">
       شمسی
          </label>
        </div>
      </li>
    </ul>
  </div>


  <div class="dropdown">
    <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
     زبان
    </button>
    <ul class="dropdown-menu">
      <li>
        <div class="form-check text-start">
          <input value="fa" class="form-check-input" type="radio" name="radioDefault" id="radioDefault"
                 checked  @input="handleInputLan"/>
          <label class="form-check-label" for="radioDefault">
            fa
          </label>
        </div>
      </li>
      <li>
        <div class="form-check text-start">
          <input value="en" class="form-check-input" type="radio" name="radioDefault" id="radioDefault"
                 checked  @input="handleInputLan"/>
          <label class="form-check-label" for="radioDefault">
            en
          </label>
        </div>
      </li>
    </ul>
  </div>

<Calendar :type="type" :lan="language" year="1404" month="6" v-model="msg"/>
  <div>{{msg}}</div>
<!--<SelectMonth/>-->
</template>

<style>



</style>
