<script setup>
import Calculator from './components/Calculator.vue';
</script>

<template>
  <div class="wrapper">
    <Calculator />
  </div>
</template>

<style scoped>
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: url('https://images.unsplash.com/photo-1466446105453-d151af699ac7?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1476&q=80') #ececec;
  background-size: cover;
}
</style>
