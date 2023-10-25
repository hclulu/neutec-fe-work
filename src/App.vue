<script setup>
import Header from './components/Header.vue'
</script>

<template>
  <div class="container">
    <header>
      <Header />
    </header>

    <main>
      
    </main>
  </div>
</template>

<style scoped>
.container{
  position: relative;
  width: 100%;
  min-height: 100%;
  max-width: 390px;
  margin: 0 auto;
  overflow-x: hidden;
}
header{
  position: relative;
  display: flex;
  height: 46px;
  padding: 0.5rem;
  background-color: #fff;
}
</style>
