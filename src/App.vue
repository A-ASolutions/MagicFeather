<template>
  <Home />
</template>

<script>
import Home from "./components/Home.vue";

export default {
  name: "App",
  components: {
    Home,
  },
};
</script>

<style>
#app {
  background-size: cover;
  background-position: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  transition: 0.4s;
  min-height: 100vh;
  padding: 25px;
}
img {
  position: relative;
  float: center;
  width: 150px;
  height: 150px;
  background-position: 50% 50%;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>

