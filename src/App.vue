<script setup>
import FenceCalculator from './components/FenceCalculator.vue'
</script>

<template>
  <div id="app">
    <FenceCalculator />
  </div>
</template>

<style>
#app {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0;
  min-height: 100vh;
  background: var( --app-bg);
  display: flex;
  justify-content: center;
  align-items: center;
  color: var(--app-color);
} 

body {
  margin: 0;
  padding: 0;
}
</style>
