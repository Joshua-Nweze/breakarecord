<template>
  <div class="container ">
    <div class="text-start row d-flex justify-content-center settings-container">
      <div class="settings-container col-lg-6 col-md-10 col-sm-12">
            <header>
              <h1>Settings</h1>
            </header>

            <div class="settings-menu">
              
              <!-- <div class="border-bottom mb-2"> Dark mode </div>
              <div class="border-bottom mb-2"> </div> -->

              <div class="form-check form-check-reverse form-switch mb-3">
                <label class="form-check-label float-start" for="">Dark mode</label>
                <input @change="$emit('toggleMode')" :checked="(mode === 'dark') ? true : false" class="form-check-input" type="checkbox" role="switch" id="darkMode">
              </div>
              <hr>

              <!-- Show animation when a record is broken, when a new record is set. -->
              <!-- <div class="form-check form-check-reverse form-switch mb-3">
                <label class="form-check-label float-start" for="">New record animation</label>
                <input @change="$emit('animation')" :checked="(showAnimation === true) ? true : false" class="form-check-input" type="checkbox" role="switch" id="animation">
              </div>
              <hr> -->
            </div>
          
       </div>
    </div>
    
  </div>
      <Footer/>
</template>

<script>
import { ref } from '@vue/reactivity'
import Footer from '@/components/Footer.vue'
export default {
    name: "Settings",
    components: { Footer },
    props: ["mode", "showAnimation"],
    emits: ["toggleMode", "animation"],

    setup() {
      
    }


}
</script>

<style>
.settings-container{
  height: 100%;
}
@media only screen and (max-width: 480px) {
  footer{
    margin-top: 300px;  
  }
  .settings-menu{
    margin-left: 10px;
    padding-top: 12px;
  }
}

@media only screen and (min-width: 481px) and (max-width: 1023px) {
  footer{
    margin-top: 230px;
  }
  .settings-menu{
    font-size: 18px;
  }
}

@media only screen and (min-width: 1024px) {  
  footer{
    margin-top: 350px;
  }
  .settings-menu{
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
  border-color: #bfbfbf;
  box-shadow: none;
  outline: 0;
  color: none;
  /* background-color: pink; */
}
</style>