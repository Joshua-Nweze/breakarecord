<template>
  <div class="container-fluid text-start row d-flex justify-content-center">
    <div class="settings-container col-lg-6 col-md-10 col-sm-12">
      <header>
        <h1>Settings</h1>
      </header>

      <div class="settings">
        
        <!-- <div class="border-bottom mb-2"> Dark mode </div>
        <div class="border-bottom mb-2"> </div> -->

        <div class="form-check form-check-reverse form-switch mb-3">
          <label class="form-check-label float-start" for="flexSwitchCheckDefault">Dark mode</label>
          <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckDefault">
        </div>
        <hr>

        <div class="form-check form-check-reverse form-switch mb-3">
          <label class="form-check-label float-start" for="flexSwitchCheckDefault">New record animation</label>
          <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckDefault">
        </div>
        <hr>

      </div>
    </div>

  </div>
    <!-- <div class="sticky-bottom">version 1.0.0</div> -->
</template>

<script>
export default {
    name: "Settings",

}
</script>

<style>
@media only screen and (max-width: 480px) {
  .settings{
    margin-left: 10px;
    padding-top: 12px;
  }
}

@media only screen and (min-width: 481px) and (max-width: 1023px) {
  .settings{
    font-size: 18px;
  }
}

@media only screen and (min-width: 1024px) {
  .settings-container{
    /* margin: auto; */
  }
  .settings{
    margin-left: 20px;
    font-size: 20px;
  }
}

.form-check-input:checked{
  border-color: #42b983;
  background-color: #42b983;
  box-shadow: none;
}

.form-check-input:focus{
  border-color: none;
  box-shadow: none;
  outline: 0;
  color: none;
  /* background-color: pink; */
}
</style>