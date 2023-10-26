<script setup>
import { ref } from 'vue'

</script>

<template>
  <div class="wrapper">
    <div class="grid">
      <div
        v-for="(item, index) in 9"
        class="item"
        :key="index"
      >
        <div :class="'box box-' + (index + 1)"></div>
        <div v-if="index == 0 || index == 2 || index == 6 || index == 8" :class="'ball ball-' + (index + 1)">0</div>
      </div>

    </div>
  </div>
</template>

<style scoped lang="scss">
.wrapper{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}
.grid{
  display: grid;
  gap: 0.25rem;
  grid-template-columns: 1fr 1fr 1fr;
  width: 100%;
  padding: 0.25rem;
  .item{
    position: relative;
    height: 100px;
  }
  .box{
    height: 100%;
    border: black solid 2px;
    background: radial-gradient(circle, rgba(113,81,95,1) 81%, rgba(0,0,0,1) 100%);
    &.box-3, &.box-5, &.box-9{
      animation: flash 0.5s linear infinite;
    }
  }
  .ball{
    position: absolute;
    z-index: 1;
    top: 50%;
    left: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 30px;
    height: 30px;
    background-color: #A5F12B;
    border-radius: 100%;
    transform: translate(-50%, -50%);
    animation: move 1.5s ease-out infinite;
  }
}

@keyframes flash {
  0% {
    opacity: 100%;
  }
  50% {
    opacity: 60%;
  }
  100% {
    opacity: 100%;
  }
}
@keyframes move {
  0% {
    left: 50%;
  }
  100% {
    left: 260%;
  }
}
</style>
