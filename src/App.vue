<script setup>
import Header from './components/Header.vue'
import Index from './components/Index.vue'
</script>

<template>
  <div class="container">
    <header>
      <Header />
    </header>

    <main>
      <Index />
    </main>
  </div>
</template>

<style scoped>
.container{
  position: relative;
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
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
main{
  flex: 1;
  background-color: #ccc;
}
</style>
