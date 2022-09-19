<template>
  <div class="container row align-items">
    <div class="col-lg-9 col-sm-12 col-md-12">

      <div class="alert alert-success alert-dismissible fade show" v-show="showRecordTitleInTimerContainer">
          <strong v-html="recordTitleInTimerContainer"></strong>
          <button type="button" class="btn-close" @click="showRecordTitleInTimerContainer = false"></button>
      </div>
      
      <span class="time" v-html="hourOne"></span><span class="time" v-html="hourTwo"></span><span class="time">:</span>
      <span class="time" v-html="minuteOne"></span><span class="time" v-html="minuteTwo"></span><span class="time">:</span>
      <span class="time" v-html="secondOne"></span><span class="time" v-html="secondTwo"></span>
      

      <div><button type="button" class="timerControl" @click="startCount"> {{ counting ? "Stop and Save" : "Start" }} </button></div>
    </div>

    <div class="col-lg-3 col-sm-12 col-md-12 records-container">
      <!-- <input type="text" v-model="newRecord">
      <button @click="b">Lets break it</button> -->
      
        <RecordsTab @setRecord="setRecord" :recordTime="recordTime"/>
    </div>

    <Footer />

  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import RecordsTab from "@/components/RecordsTab.vue"
import Footer from "@/components/Footer.vue"
import { computed } from '@vue/runtime-core';
export default {
  name: "Timer",
  components: { RecordsTab, Footer },

  setup() {
    let hourOne = ref(0);
    let hourTwo = ref(0);
    let minuteOne = ref(0);
    let minuteTwo = ref(0);
    let secondOne = ref(0);
    let secondTwo  = ref(0);

    let input = ref(null);


    let recordTitleInTimerContainer = ref(null);
    let showRecordTitleInTimerContainer = ref(false)

    let counting = ref(false)

    let recordTime = ref(`${hourOne.value}${hourTwo.value}:${minuteOne.value}${minuteTwo.value}:${secondOne.value}${secondTwo.value}`);

    function startCount() {
        counting.value =! counting.value;
        setInterval(() => {
          if (!recordTitleInTimerContainer) {
            alert("Enter record first");
            console.log(2132443);

            return;
          }
        secondTwo .value++;

        if (secondTwo.value == 10) {
          secondTwo.value = 0;
          secondOne.value++;
        }
        if (secondOne.value == 6) {
          secondOne.value = 0;
          minuteTwo.value++;
        }

        if (minuteTwo.value == 10) {
          minuteTwo.value = 0;
          minuteOne.value++;
        }
        if (minuteOne.value == 6) {
          minuteOne.value = 0; 
          hourTwo.value++;
        }

        if (hourTwo.value == 10) {
          hourTwo.value = 0; 
          hourOne.value++;
        }
      }, 1000)
      
    }

    function setRecord(record) {
      let regex = /^\s*$/;

      if (!regex.test(record)){
        recordTitleInTimerContainer.value = record;
        showRecordTitleInTimerContainer.value = true;
      }

    }
        
     



    return { hourOne, hourTwo, minuteOne, minuteTwo, secondOne, secondTwo , startCount, recordTitleInTimerContainer,  input, setRecord, recordTime, counting, showRecordTitleInTimerContainer }
  }

  
}
</script>

<style scoped>

    /* *{
      color: #6d7276;
    } */

    @media only screen and (max-width: 480px) {
     .time{
      font-size: 50px;
     }
    }

    @media only screen and (min-width: 481px) and (max-width: 1023px) {
       .time{
          font-size: 100px;
       }
    }

    @media only screen and (min-width: 1024px) {
        .time{
          font-size: 100px;
        }
        .records-container{
          border-left: 1px solid ;
        }
    }

    .timerControl{
      background-color: #42b983;
      border-radius: 2px;
      border: none;
      padding: 5px;
    }
/* 
    button:hover{
      background-color: #42b983;
      color: black;
      border: #42b983;
    } */
</style>