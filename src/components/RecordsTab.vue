<template>

    <div class="text-start pt-2">
        <span class="pt-1 records-header">Records</span>
        
        <button type="button" class="btn float-end" :style="{color: addBtnColor}" @click="toggleShowAddRecord">{{ showAddRecord ? "Close" : "Set Record" }}</button>
    </div>

    <div class="input-group mb-3 mt-3" v-show="showAddRecord">
        <input v-model="newRecord" type="text" class="form-control" placeholder="Enter a new record">
        <button @click="setRecord" class="input-group-text">Set</button>
    </div>

    <RecordList :recordTitle="recordTitle"/>
</template>

<script>
import { ref } from '@vue/reactivity'
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
        // let addBtnColor = computed (() => {
        //     return showAddRecord ? "black" : "red"
        // }) 

        function setRecord() {
            ctx.emit("setRecord", newRecord.value);
            // if (newRecord.value !== "") {
            //     arr.push(newRecord.value);
            // }
            recordTitle.value = newRecord.value;
            // console.log(newRecord.value);
            newRecord.value = "";
        }

        function toggleShowAddRecord() {
            showAddRecord.value =! showAddRecord.value;
            if (showAddRecord) {
                addBtnColor.value = "green";
            } else if (showAddRecord.value = flase) {
                addBtnColor.value = "pink"
            }
        }


        return { newRecord, setRecord, showAddRecord, toggleShowAddRecord, addBtnColor, arr, recordTitle }
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