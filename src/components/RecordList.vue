<template>
  <div class="record-list-container">
    <div v-for="(record, index) in recordDetails" :key="index" class="card text-start mt-3">
        <div class="card-header"> 
            <span @click="selectRecord(record)" class="select">{{record.name}}</span>
            
            <i class="bi bi-three-dots-vertical dropdown-toggle dropdown-icon float-end" data-bs-toggle="dropdown"></i>
    
            <ul class="dropdown-menu">
                <li @click="selectRecord(record)"><a class="dropdown-item" href="#"><i class="bi bi-hand-index-thumb"></i>  Select</a></li> 
                <li @click="editRecord"><a class="dropdown-item" href="#"><i class="bi bi-pencil-square"></i>  Edit</a></li> 
                <li @click="deleteRecord(record, index)"><a class="dropdown-item text-danger" href="#"><i class="bi bi-trash3"></i>  Delete</a></li>
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
    props: [ "recordTitle", "recordDetails" ],

    setup (props, ctx) {
        // console.log(props.recordTitle);

        function selectRecord(record) {
            ctx.emit("selectRecord", record.name);
            // console.log(record.name);
        }


        function deleteRecord(record, index) {

            alert(`Are you sure you want to delete "${record.name}" from records`);
            props.recordDetails.splice(index, 1);

            ctx.emit("hideRecordTitle")
            
        }

        return { selectRecord, deleteRecord }
    },

    emits: [ "selectRecord", "hideRecordTitle" ]
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
    }
</style>