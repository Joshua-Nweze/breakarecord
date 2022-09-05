<template>
  <nav>
    <router-link to="/"><i class="bi bi-house-door-fill nav-icon"></i></router-link>
    <router-link to="/settings"><i class="bi bi-gear-fill nav-icon"></i></router-link>
  </nav>
  <router-view/>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #6d7276;
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
