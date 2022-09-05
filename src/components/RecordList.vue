<template>
  <div>
    <div class="card text-start">
        <div class="card-header">Featured 
            <span class="dropdown float-end">

    <i class="bi bi-three-dots-vertical dropdown-toggle" data-bs-toggle="dropdown"></i>
    
    <ul class="dropdown-menu">
        <li><a class="dropdown-item" href="#"><i class="bi bi-pencil-square"></i>  Edit</a></li> 
        <li><a class="dropdown-item text-danger" href="#"><i class="bi bi-trash3"></i>  Delete</a></li>
    </ul>
            </span>
        </div>
        <div class="card-body">
            <p class="card-text">Record Info</p>
        </div>
    </div>

    
  </div>
</template>

<script>
export default {
    name: "RecordList",
}
</script>

<style>
.dropdown-toggle::after {
    display: none;
}
</style>