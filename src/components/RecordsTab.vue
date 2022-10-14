<template>

    <!-- Input area to add records -->
    <div class="text-start pt-2">
        <span class="pt-1 records-header">Records</span>

        
        <button type="button" class="btn float-end" :style="{color: addBtnColor, 'border-color': btnBorderColor}" @click="toggleShowAddRecord">{{ showAddRecord ? "Close" : "Set Record" }}</button>


        <div class="alert alert-warning alert-dismissible fade show" role="alert" v-show="warning">
            <strong v-html="warningMessage"></strong>
            <button type="button" @click="warning = false" class="btn-close"></button>
        </div>
    </div>

    <!-- Records details -->
    <div class="mb-3 mt-3" v-show="showAddRecord">
            <div class="input-group">
                <input v-model="newRecord" @keypress.enter="setRecord" type="text" class="form-control" placeholder="Enter a new record">
                <button @click="setRecord" type="submit" class="input-group-text">Set</button>
            </div>
    </div>

    <div v-if="noRecord">No record to show</div>

    <RecordList
        :recordTitle="recordTitle"
        :recordDetails="recordDetails" 
        @selectRecord="selectRecord" 
        @hideRecordTitle="hideRecordTitle"
        @deleteRecord="deleteRecord"
        :mode="mode"
        @editRecordName="editRecordName"
        :showAnimation="showAnimation"
        :getRecordTime="getRecordTime"
        :recordTime="recordTime"
    />

</template>




<script>
import { reactive, ref } from '@vue/reactivity'
import RecordList from '@/components/RecordList.vue'
import { computed, onUpdated } from '@vue/runtime-core'

export default {
    name: "RecordTab",
    props: [ "recordTime", "newRecordTime", "mode", "allowUpdateRecordTime", "recordTitleInTimerContainer", "showAnimation", "getRecordTime" ],
    //NOTE: recordTimeGetter was removed from the emits array and RecordList tag
    emits: ["setRecord", "selectRecord", "hideRecordTitle", "editRecordName"],
    components: {
        RecordList
    },

    setup (props, ctx) {
        let newRecord = ref("");
        let showAddRecord = ref(false);
        let noRecord = ref(true);

        let btnColor = ref("#42b983")

        let addBtnColor = ref(btnColor);
        let btnBorderColor  = ref(btnColor)
        let recordTitle = ref("");
        let warning = ref(false);
        let warningMessage = ref("");

        let recordDetails = reactive([
            {
                name : "beans time",
                time : 279
            }
         ])

        function selectRecord(record) {
            ctx.emit("selectRecord", record)
            // console.log(record);
        }

        function hideRecordTitle() {
            ctx.emit("hideRecordTitle")
        }

        function setRecord() {
            // e.preventDefault();
            ctx.emit("setRecord", newRecord.value);
            let regex = /^\s*$/;
            if (!regex.test(newRecord.value)) {

                // Method one of checking if record exists

                // for (const j in recordDetails) {
                //     if (Object.hasOwn(recordDetails, j)) {
                //         const record = recordDetails[j];
                //         console.log(record);
                //         if (newRecord.value == record.name) {
                //             warningMessage.value = `"${newRecord.value}" already exists`;
                //             warning.value = true;

                //             return
                //         }
                //     }
                // }

                // Method one of checking if record exists, this is easier

                for (const record of recordDetails) {
                    if (newRecord.value == record.name) {
                            warningMessage.value = `"${newRecord.value}" already exists`;
                            warning.value = true;

                            return
                        }
                }

                // Adding a new record to the record details array
                const NEW_RECORD = reactive({
                    
                    name : newRecord.value,
                    time : props.recordTime

                })

                recordDetails.unshift(NEW_RECORD);

                recordTitle.value = newRecord.value;
                console.log(newRecord.value);
                newRecord.value = "";

                console.log(recordDetails);

                if (recordDetails.length > 0) {
                    noRecord.value = false;
                }

                recordDetails.time = props.recordTime;

            } else {
                warningMessage.value = "Enter valid record";
                warning.value = true;
                console.log(newRecord.value);
            }

        }

        function deleteRecord(index) {
            // alert(`Are you sure you want to delete "${record.name}" from records`);
            recordDetails.splice(index, 1);

            if (recordDetails.length == 0) {
                    noRecord.value = true;
                }
        }

        function toggleShowAddRecord() {
            showAddRecord.value =! showAddRecord.value;
            warning.value = false;

            if (showAddRecord.value) {
                btnColor.value = "red";
            } else if (!showAddRecord.value) {
                btnColor.value = "#42b983"
            }

        }

        function editRecordName({updatedRecordName, record}){
            record.name = updatedRecordName.value;
            ctx.emit("editRecordName")
        }

        // function recordTimeGetter() {
        //     ctx.emit("recordTimeGetter")
        // }

    
        onUpdated(() => {
            if(props.allowUpdateRecordTime){
                //  Method one of updating record time

                // for (const j in recordDetails) {
                //     if (Object.hasOwn(recordDetails, j)) {
                //         const record = recordDetails[j];
                //         // console.log(record);
                //         if (props.recordTitleInTimerContainer == record.name) {
                //             console.log(record);
                //             record.time = props.newRecordTime;
                //         }
                //     }
                // }

                
                //  Method two of updating record time, this is easier

                for (const record of recordDetails) {
                    if (props.recordTitleInTimerContainer == record.name) {
                        record.time = props.newRecordTime;
                    }
                }
            }

        //   (function() {
        //     for (const record of recordDetails) {
        //         // console.log(props.recordTime);
        //         if (typeof props.recordTime == 'string') {
        //             console.log("stringf");
        //         } else {
        //             console.log("what!!");
        //         }
        //     }
        //   })();
        })

        //recordTimeGetter was removed from the return object

        return { newRecord, setRecord, showAddRecord, toggleShowAddRecord, addBtnColor, recordTitle, warning, recordDetails, warningMessage, noRecord, selectRecord, btnBorderColor, btnColor, hideRecordTitle, deleteRecord, editRecordName };
    },

}
</script>





<style scoped>
    *{
        transition: ease-out 0.5s ;
    }
    
    .form-control:focus{
        border-color: #42b983;
        box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px #42b983;
    }

    .records-header{
        font-size: 30px;
    }
</style>