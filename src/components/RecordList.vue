<template>
  <div class="record-list-container">
    <div v-for="(record, index) in recordDetails" :key="index" class="card text-start mt-3" :class="(mode === 'dark') ? 'dark-theme' : 'light-theme'">
        <div class="card-header"> 
            <span @click="selectRecord(record)" class="select">{{record.name}}</span>
            
            <i class="bi bi-three-dots-vertical dropdown-toggle dropdown-icon float-end" data-bs-toggle="dropdown"></i>
    
            <ul class="dropdown-menu" :class="(mode === 'dark') ? 'dark-theme' : 'light-theme'">
                <li @click="selectRecord(record)"><span :class="(mode === 'dark') ? 'dark-theme' : 'light-theme'"  class="dropdown-item" href="#"><i class="bi bi-hand-index-thumb"></i>  Select</span></li> 
                <li @click="editRecord"><span :class="(mode === 'dark') ? 'dark-theme' : 'light-theme'"  class="dropdown-item" href="#"><i class="bi bi-pencil-square"></i>  Edit</span></li> 
                <li @click="deleteRecord(index)"><span class="dropdown-item text-danger" href="#"><i class="bi bi-trash3"></i>  Delete</span></li>
            </ul>
            
        </div>
        <div class="card-body">
            <p class="card-text">Last record: {{record.time}}</p>
        </div>
    </div>

    
  </div>
</template>

<script>
import { ref } from '@vue/reactivity';
export default {
    name: "RecordList",
    props: [ "recordTitle", "recordDetails", "mode" ],

    setup (props, ctx) {
        // console.log(props.recordTitle);

        function selectRecord(record){

            ctx.emit("selectRecord", record);
            
        }


        function deleteRecord(index) {

            ctx.emit("deleteRecord", index)
            ctx.emit("hideRecordTitle")
            
        }

        return { selectRecord, deleteRecord }
    },

    emits: [ "selectRecord", "hideRecordTitle", "deleteRecord" ]
}
</script>

<style>
    .dropdown-toggle::after {
        display: none;
    }

    .dropdown-icon:hover{
        cursor: pointer;
    }

    .select:hover{
        cursor: pointer;
    }

    .record-list-container{
        height: 380px;
        overflow-y: scroll;
        padding-right: 5px;
        /* background-color: inherit; */
    }

    .dark-theme{
        background-color: #36383d;
        color: #899197;
    }
</style>