<template>
  <div class="container row">
    <div class="col-lg-9 col-sm-12 col-md-12 time-container">

      <div class="recordTitle" v-html="recordTitleInTimerContainer"></div>
      
      <span class="time" v-html="hourOne"></span><span class="time" v-html="hourTwo"></span><span class="time">:</span>
      <span class="time" v-html="minuteOne"></span><span class="time" v-html="minuteTwo"></span><span class="time">:</span>
      <span class="time" v-html="secondOne"></span><span class="time" v-html="secondTwo"></span>
      

      <div><button type="button" class="timerControll" @click="startCount">Let's break that record</button></div>
    </div>

    <div class="col-lg-3 col-sm-12 col-md-12">
      <!-- <input type="text" v-model="newRecord">
      <button @click="b">Lets break it</button> -->
      
        <RecordsTab @setRecord="setRecord"/>
    </div>

    
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import RecordsTab from "@/components/RecordsTab.vue"
import { computed } from '@vue/runtime-core';
export default {
  name: "Timer",
  components: { RecordsTab },

  setup() {
    let hourOne = ref(0);
    let hourTwo = ref(0);
    let minuteOne = ref(0);
    let minuteTwo = ref(0);
    let secondOne = ref(0);
    let secondTwo  = ref(0);

    let input = ref(null);

    let recordTitleInTimerContainer = ref(null);

    // let newRecord = ref("");

    let recordTime = ref(`${hourOne.value}${hourTwo.value}:${minuteOne.value}${minuteTwo.value}:${secondOne.value}${secondTwo.value}`);
    // let emptyInputWarning = ref(null)

    // let getNewRecord = computed((value) => {
    //     newRecord.value = value
    // })

    // function setRecord (val) {
    //     newRecord.value = val;
    //   recordTitleInTimerContainer.value = newRecord.value;
    // }

    function startCount() {
      setInterval(() => {
        secondTwo .value++;

        if (secondTwo .value == 10) {
          secondTwo .value = 0;
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
      // if (record == "") {
      //   // emptyInputWarning.value = "Enter valid record";
      //   // warning = true;
      //   alert(1234567890)
      //   document.getElementById('warning').classList.add = "show"
      // } else {
      // }
        // warning = false;
      recordTitleInTimerContainer.value = record;
    }


    return { hourOne, hourTwo, minuteOne, minuteTwo, secondOne, secondTwo , startCount, recordTitleInTimerContainer,  input, setRecord, recordTime }
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
        .time-container{
          border-right: 1px solid ;
        }
    }

    .timerControll{
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