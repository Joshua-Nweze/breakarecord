<template>
    <div class="input-group mb-3 mt-3">
        <input v-model="newRecord" type="text" class="form-control" placeholder="Enter a new record">
        <button 
        @click="setRecord"
        class="input-group-text">Set</button>
    </div>

    <RecordList />
</template>

<script>
import { ref } from '@vue/reactivity'
import RecordList from '@/components/RecordList.vue'

export default {
    name: "RecordTab",
    props: [ "get"],
    components: {
        RecordList
    },

    setup (props, ctx) {
        let newRecord = ref("")

        function setRecord() {
            ctx.emit("setRecord", newRecord.value);
            newRecord.value = "";
        }

        return { newRecord, setRecord }
    },

    emits: ["setRecord"]

}
</script>

<style scoped>
    .form-control:focus{
  border-color: #42b983;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px #42b983;

    }
</style>