<template>

    <!-- Input area to add records -->
    <div class="text-start pt-2">
        <span class="pt-1 records-header">Records</span>

        
        <button type="button" class="btn float-end" :style="{color: addBtnColor}" @click="toggleShowAddRecord">{{ showAddRecord ? "Close" : "Set Record" }}</button>


        <div class="alert alert-warning alert-dismissible fade show" role="alert" v-show="warning" id="warning">
            <strong> Enter valid record </strong>
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

    <RecordList :recordTitle="recordTitle" :recordDetails="recordDetails"/>
</template>

<script>
import { reactive, ref } from '@vue/reactivity'
import RecordList from '@/components/RecordList.vue'
import { computed } from '@vue/runtime-core'

export default {
    name: "RecordTab",
    // props: [ "get"],
    components: {
        RecordList
    },

    setup (props, ctx) {
        let newRecord = ref("");
        let showAddRecord = ref(true);

        let addBtnColor = ref("pink")
        let arr = ref([]);
        let recordTitle = ref("")
        let warning = ref(false)
        // let addBtnColor = computed (() => {
        //     return showAddRecord ? "black" : "red"
        // }) 

        let recordDetails = ref([
                {
                    name : "beans time",
                    time : 279
                }
         ])

        recordDetails.value.push(
            {
                name : "beans",
                time: 4567
            }
            
        )



        function setRecord() {
            // e.preventDefault();
            ctx.emit("setRecord", newRecord.value);
            if (newRecord.value == "") {
                warning.value = true;
                // return;
            } else {
                const NEW_RECORD = reactive({
                    
                    id : Math.floor(Math.random() * 10000000),
                    name : newRecord.value,
                    time : Math.floor(Math.random() * 10000000)
                    
            })

                recordDetails.value.unshift(NEW_RECORD);

                recordTitle.value = newRecord.value;
                console.log(newRecord.value);
                newRecord.value = "";
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

        return { newRecord, setRecord, showAddRecord, toggleShowAddRecord, addBtnColor, arr, recordTitle, warning, recordDetails }
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