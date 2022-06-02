<template>
<div class="backgrounds">
  <div class="background">
    <div class="background-item">
    <div class="background-overlay"></div>
    </div>
  </div>
</div>
  <Dashboard />
</template>

<script>
import Dashboard from "@/components/Dashboard.vue";

export default {
  name: "App",
  components: {
    Dashboard,
  },
};
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

.background-item {
  background-image: url(./assets/defaultBG.jpg);
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: transparent;
    background-size: cover;
    background-position: center center;
    background-repeat: no-repeat;
}

.background {
  position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: transparent;
    background-size: cover;
    background-position: center center;
    background-repeat: no-repeat;
    margin: 0;
    padding: 0;
    z-index: 1;
}

  .background-overlay {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 2;
    background-image: url(./assets/background.png);
    background-size: 100% 100%;
    opacity: 0.4;
  }
</style>
