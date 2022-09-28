<template>
<div :class="mode">
  <nav>
    <router-link to="/"><i class="bi bi-house-door-fill nav-icon"></i></router-link>  
    <router-link to="/settings"><i class="bi bi-gear-fill nav-icon"></i></router-link>
  </nav>
  <router-view :mode="mode" @toggleMode="toggleMode"/>
</div>
  
</template>

<script>
import { ref } from '@vue/reactivity'
export default {

  setup() {
    let mode = ref("light");

    function toggleMode() {
      if (mode.value === "light") {
        mode.value = "dark"
      } else {
        mode.value = "light"
      }
    }

    return { mode, toggleMode }
  },

  emits: ["toggleMode"]
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #6d7276;
  /* height: 100%; */
  /* min-height: 100%; */
  /* transition: background 0.3s ease-in-out; */
}

.light{
  transition: ease-in 1s;
}

.dark{
  background-color: #202124;
  height: 100%;
  /* min-height: 100%; */
  transition: ease-in 1s;
  /* color: #6d7276; */
}

nav {
  padding: 30px;  
}

nav a {
  font-weight: bold;
  color: #6d7276;
}

nav a:hover{
  color: #42b983;
}

nav a.router-link-exact-active {
  color: #42b983;
}

.nav-icon{
  font-size: 30px;
  margin: 0 5px;
}

</style>
