<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <PatientsList  msg="Liste des patients"/>
</template>

<script>
import PatientsList from "./components/PatientsList.vue";

export default {
  name: 'App',
  components: {
    PatientsList
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
