<template>
<div class="container">
  <div class="row d-flex justify-content-center">
    <div class="col-lg-8 col-md-7 col-sm-12">

      <div :class="{ 'alert-success': success, 'alert-danger': dangerWarning }"  class="alert alert-dismissible fade show" v-show="showRecordTitleInTimerContainer">
          <strong v-html="recordTitleInTimerContainer"></strong>
          <strong v-html="recordTitleError" v-if="dangerWarning"></strong>
          <button type="button" class="btn-close" @click="closeRecordTitle"></button>
      </div>
      
      <span class="time" v-html="hourOne"></span><span class="time" v-html="hourTwo"></span><span class="time">:</span>
      <span class="time" v-html="minuteOne"></span><span class="time" v-html="minuteTwo"></span><span class="time">:</span>
      <span class="time" v-html="secondOne"></span><span class="time" v-html="secondTwo"></span>

      <div style="z-index:999">
        {{ recordTime }}
      </div>
      

      <div><button type="button" :class="{}" class="timerControl" @click="startCount"> {{ counting ? "Stop and Save" : "Start" }} </button></div>

    </div>
    <div class="col-lg-4 col-md-5 col-sm-12 border-start">
      
        <RecordsTab @setRecord="setRecord" :recordTime="recordTime" @selectRecord="selectRecord"/>
    </div>
  </div>
</div>
  <Footer />

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
    let recordTitleError = ref(null);
    let showRecordTitleInTimerContainer = ref(false)

    let counting = ref(false)

    let recordTime = ref(`${hourOne.value}${hourTwo.value}:${minuteOne.value}${minuteTwo.value}:${secondOne.value}${secondTwo.value}`);

    let success = ref(true);
    let dangerWarning = ref(false)

    function closeRecordTitle() {
      showRecordTitleInTimerContainer.value = false; 
      recordTitleInTimerContainer.value = null;
    }

    function startCount() {
        counting.value =! counting.value;

          if (recordTitleInTimerContainer.value == null) {
            recordTitleError.value = "Enter or click record name before counting!";
            dangerWarning.value = true;
            showRecordTitleInTimerContainer.value = true;
            counting.value = false; 
            // recordTitleInTimerContainer.value = null

            return false;
          }

          if (counting.value) {
            let i = setInterval(() => {

              secondTwo.value++;

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

              
              if (!counting.value) {
                clearInterval(i);
                recordTime.value = `${hourOne.value}${hourTwo.value}:${minuteOne.value}${minuteTwo.value}:${secondOne.value}${secondTwo.value}`;
                console.log(recordTime.value);
              }
            }, 1000)

          } 
          // else if (!counting.value) {
          //   clearInterval(i);
          //   console.log('hi');
          // }
      
      
    }

    function setRecord(record) {
      let regex = /^\s*$/;

      if (!regex.test(record)){
        success.value = true;
        dangerWarning.value = false;
        recordTitleInTimerContainer.value = record;
        showRecordTitleInTimerContainer.value = true;
      }

    }

    function selectRecord(record) {
      recordTitleInTimerContainer.value = record;
      showRecordTitleInTimerContainer.value = true;
      dangerWarning.value = false;
      success.value = true;
    }

    return { hourOne, hourTwo, minuteOne, minuteTwo, secondOne, secondTwo , startCount, recordTitleInTimerContainer, recordTitleError, input, setRecord, recordTime, counting, showRecordTitleInTimerContainer, success, dangerWarning, selectRecord, closeRecordTitle }
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