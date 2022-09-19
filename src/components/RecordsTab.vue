<template>

    <!-- Input area to add records -->
    <div class="text-start pt-2">
        <span class="pt-1 records-header">Records</span>

        
        <button type="button" class="btn float-end" :style="{color: addBtnColor}" @click="toggleShowAddRecord">{{ showAddRecord ? "Close" : "Set Record" }}</button>


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

    <RecordList :recordTitle="recordTitle" :recordDetails="recordDetails"/>

</template>





<script>
import { reactive, ref } from '@vue/reactivity'
import RecordList from '@/components/RecordList.vue'
import { computed } from '@vue/runtime-core'

export default {
    name: "RecordTab",
    props: [ "recordTime" ],
    components: {
        RecordList
    },

    setup (props, ctx) {
        let newRecord = ref("");
        let showAddRecord = ref(false);
        let noRecord = ref(true);

        let addBtnColor = ref("pink");
        let recordTitle = ref("");
        let warning = ref(false);
        let warningMessage = ref("");
        // let addBtnColor = computed (() => {
        //     return showAddRecord ? "black" : "red"
        // }) 

        let recordDetails = reactive([
            // {
            //     name : "beans time",
            //     time : 279
            // }
         ])


        function setRecord() {
            // e.preventDefault();
            ctx.emit("setRecord", newRecord.value);
            let regex = /^\s*$/;
            if (!regex.test(newRecord.value)) {

                for (const j in recordDetails) {
                    if (Object.hasOwn(recordDetails, j)) {
                        const record = recordDetails[j];
                        console.log(record);
                        if (newRecord.value == record.name) {
                            warningMessage.value = `"${newRecord.value}" already exists`;
                            warning.value = true;

                            return
                        }
                    }
                }

                const NEW_RECORD = reactive({
                    
                    
                    name : newRecord.value,
                    time : props.recordTime
                    
                })

                recordDetails.unshift(NEW_RECORD);

                recordTitle.value = newRecord.value;
                console.log(newRecord.value);
                newRecord.value = "";

                if (recordDetails.length > 0) {
                    noRecord.value = false;
                }

            } else {
                warningMessage.value = "Enter valid record";
                warning.value = true;
                console.log(newRecord.value);
            }

        }


        function toggleShowAddRecord() {
            showAddRecord.value =! showAddRecord.value;
            warning.value = false;
            if (showAddRecord) {
                addBtnColor.value = "green";
            } else if (showAddRecord.value = flase) {
                addBtnColor.value = "pink"
            }
        }


        return { newRecord, setRecord, showAddRecord, toggleShowAddRecord, addBtnColor, recordTitle, warning, recordDetails, warningMessage, noRecord };
    },

    emits: ["setRecord"]

}
</script>





<style scoped>
    .form-control:focus{
        border-color: #42b983;
        box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px #42b983;
    }

    .records-header{
        font-size: 30px;
    }
</style>