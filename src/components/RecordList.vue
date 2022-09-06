<template>
  <div>
    <div class="card text-start mt-3">
        <div class="card-header">Featured {{recordTitle}}
            <span class="dropdown float-end">
                <i class="bi bi-three-dots-vertical dropdown-toggle dropdown-icon" data-bs-toggle="dropdown"></i>
        
                <ul class="dropdown-menu">
                    <li><a class="dropdown-item" href="#"><i class="bi bi-pencil-square"></i>  Edit</a></li> 
                    <li><a class="dropdown-item text-danger" href="#"><i class="bi bi-trash3"></i>  Delete</a></li>
                </ul>
            </span>
        </div>
        <div class="card-body">
            <p class="card-text">Last record: recordTime</p>
        </div>
    </div>

    
  </div>
</template>

<script>
export default {
    name: "RecordList",
    props: [ "recordTitle" ],

    setup (props) {
        console.log(props.recordTitle);
    }
}
</script>

<style>
    .dropdown-toggle::after {
        display: none;
    }

    .dropdown-icon:hover{
        cursor: pointer;
    }
</style>