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
  color: #2c3e50;
  margin: 0;
  /* padding: 20px;  */
  min-height: 100vh;
  /* background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); */
  background: linear-gradient(135deg, #8da0f8 0%, #9871c0 100%);
  display: flex;
  justify-content: center;
  align-items: center;
} 

body {
  margin: 0;
  padding: 0;
}
</style>
