<template>
  <div class="container">
    <div class="row d-flex justify-content-center">
      <div class="col-lg-8 col-md-7 col-sm-12">

        <div :class="{ 'alert-success': success, 'alert-danger': dangerWarning }"  class="alert alert-dismissible fade show" v-show="showRecordTitleInTimerContainer">
            <strong v-html="recordTitleInTimerContainer"></strong>
            <strong v-html="recordTitleError" v-if="dangerWarning"></strong>
            <button type="button" class="btn-close" @click="hideRecordTitle"></button>
        </div>
        
        <span>
          <span class="time" v-html="hourOne"></span><span class="time" v-html="hourTwo"></span><span class="time">:</span>
          <span class="time" v-html="minuteOne"></span><span class="time" v-html="minuteTwo"></span><span class="time">:</span>
          <span class="time" v-html="secondOne"></span><span class="time" v-html="secondTwo"></span>
        </span>

        <div><button
          type="button"
          class="timerControl" 
          style="color: ghostwhite"
          @click="startCount"
          :data-bs-toggle="(counting === true) ? 'modal' : ''" 
          :data-bs-target="(counting === true) ? '#recordTimeModal' : ''"> 
          {{ counting ? "Stop" : "Start" }} 
          </button></div>

      </div>
      <div class="col-lg-4 col-md-5 col-sm-12 border-start">
        

        <!-- recordTimeGetter custom event has been removed from RecordsTab -->
          <RecordsTab 
           @setRecord="setRecord"
           :recordTime="recordTime" 
           @selectRecord="selectRecord" 
           :newRecordTime="newRecordTime"
           @hideRecordTitle="hideRecordTitle"
           :mode="mode" 
           :allowUpdateRecordTime="allowUpdateRecordTime" 
           :recordTitleInTimerContainer="recordTitleInTimerContainer"
           @editRecordName="editRecordName"
           :showAnimation="showAnimation"
           :getRecordTime="getRecordTime"
           />
      </div>
    </div>
  </div>
  <Footer/>


  <!--New record modal -->
  <div class="modal fade" id="recordTimeModal" tabindex="-1" aria-labelledby="recordTimeModalLabel" aria-hidden="true" data-bs-backdrop="static" data-bs-keyboard="false">
    <div class="modal-dialog">
      <div class="modal-content" :class="(mode === 'dark') ? 'dark-theme' : 'light-theme' ">
        <div class="modal-header" style="border-color: #42b983">
          <h5 class="modal-title" id="exampleModalLabel" style="color: #42b983">New record time</h5>
          <button type="button" class="btn-close" style="color: red" data-bs-dismiss="modal" aria-label="Close" @click="recordTimeModalClosed"></button>
        </div>
        <div class="modal-body">
          <span class="fs-5" v-html="record_name"></span> <br>
          <span class="fs-5">{{ recordTime }}</span>
        </div>
        <div class="modal-footer border-0">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="recordTimeModalClosed">Cancel</button>
          <button type="button" class="btn" style="background: #42b983; color: ghostwhite" data-bs-dismiss="modal" @click="updateRecordTime">Save changes</button>
        </div>
      </div>
    </div>
  </div>


</template>

<script>
import { ref } from '@vue/reactivity'
import RecordsTab from "@/components/RecordsTab.vue"
import Footer from "@/components/Footer.vue"
import { onUpdated } from '@vue/runtime-core'
export default {
  name: "Timer",
  components: { RecordsTab, Footer },
  props: ["mode", "showAnimation"],
  emits: ["animation", "editRecordName"],

  setup() {
    let hourOne = ref(0);
    let hourTwo = ref(0);
    let minuteOne = ref(0);
    let minuteTwo = ref(0);
    let secondOne = ref(0);
    let secondTwo  = ref(0);

    let recordTitleInTimerContainer = ref(null);
    let recordTitleError = ref(null);
    let showRecordTitleInTimerContainer = ref(false)

    let counting = ref()

    let recordTime = ref(`${hourOne.value}${hourTwo.value}:${minuteOne.value}${minuteTwo.value}:${secondOne.value}${secondTwo.value}`);
    let newRecordTime = ref(null)

    let success = ref(true);
    let dangerWarning = ref(false);

    let allowUpdateRecordTime = ref(null);

    let record_name = ref();
    let getRecordTime = ref(false);

    function startCount() {
        counting.value =! counting.value;
        // getRecordTime.value =! getRecordTime.value;

          if (recordTitleInTimerContainer.value == null) {
            recordTitleError.value = "Enter or click/select record before counting!";
            dangerWarning.value = true;
            showRecordTitleInTimerContainer.value = true;
            counting.value = false;
            // recordTitleInTimerContainer.value = null

            return false;
          }


          if (counting.value) {
            getRecordTime.value = true;
            // console.log(getRecordTime.value);
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
                // recordTime.value = `${hourOne.value}${hourTwo.value}:${minuteOne.value}${minuteTwo.value}:${secondOne.value}${secondTwo.value}`;
              }

              recordTime.value = `${hourOne.value}${hourTwo.value}:${minuteOne.value}${minuteTwo.value}:${secondOne.value}${secondTwo.value}`;
            }, 1000)

          }
      
    }

    function resetTimer() {
      hourOne.value = 0;
      hourTwo.value = 0;
      minuteOne.value = 0;
      minuteTwo.value = 0;
      secondOne.value = 0;
      secondTwo.value = 0;

      recordTime.value = `${hourOne.value}${hourTwo.value}:${minuteOne.value}${minuteTwo.value}:${secondOne.value}${secondTwo.value}`;
    }

    function setRecord(recordName) {
      let regex = /^\s*$/;

      if (!regex.test(recordName)){
        record_name.value = recordName;
        success.value = true;
        dangerWarning.value = false;
        recordTitleInTimerContainer.value = recordName;
        showRecordTitleInTimerContainer.value = true;
      }

    }

    // function closeRecordTitle() {
    //   showRecordTitleInTimerContainer.value = false;
    //   recordTitleInTimerContainer.value = null;
    //   getRecordTime.value = false
    //   counting.value = false;
      
    //   // resetTimer()
    // }

    function hideRecordTitle() {
      showRecordTitleInTimerContainer.value = false;
      recordTitleInTimerContainer.value = null;
      getRecordTime.value = false;
      counting.value = false;

      resetTimer();
    }

    function selectRecord(record) {
      record_name.value = record.name
      recordTitleInTimerContainer.value = record.name;
      showRecordTitleInTimerContainer.value = true;
      dangerWarning.value = false;
      success.value = true;
      getRecordTime.value = true;

      console.log(getRecordTime.value);
    }

    function recordTimeModalClosed () {
      resetTimer()

      console.log(recordTime.value);
    }

    
    function updateRecordTime () {
        newRecordTime.value = recordTime.value
        allowUpdateRecordTime.value = true;

        console.log('from update record', recordTitleInTimerContainer.value, recordTime.value);

    }

    function editRecordName () {
      recordTitleInTimerContainer.value = null;
      showRecordTitleInTimerContainer.value = false
      counting.value = false;

      resetTimer();
    }

    onUpdated(() => {
      // console.log(12345678);
      // function recordTimeGetter() {
      //   console.log(2345);
      //   // console.log(recordTime.value);
      // }

      // recordTimeGetter()
    })

    return { hourOne, hourTwo, minuteOne, minuteTwo, secondOne, secondTwo , startCount, recordTitleInTimerContainer, recordTitleError, setRecord, recordTime, counting, showRecordTitleInTimerContainer, success, dangerWarning, selectRecord, hideRecordTitle, newRecordTime, hideRecordTitle, updateRecordTime, record_name, recordTimeModalClosed, allowUpdateRecordTime, resetTimer, editRecordName, getRecordTime}
  }

  
}
</script>

<style scoped>


    @media only screen and (max-width: 480px) {
      footer{
        margin-top: 15px;
      }
     .time{
      font-size: 50px;
     }
    }

    @media only screen and (min-width: 481px) and (max-width: 1023px) {
        footer{
          margin-top: 40px;
        }
       .time{
          font-size: 100px;
       }
    }

    @media only screen and (min-width: 1024px) {
        footer{
          margin-top: 60px;
        }
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

    .dark-theme{
        background-color: #36383d;
        color: #b7bdc0;
    }
/* 
    button:hover{
      background-color: #42b983;
      color: black;
      border: #42b983;
    } */

</style>