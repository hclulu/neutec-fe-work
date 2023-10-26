<script setup>
import { ref } from 'vue'
import { vOnClickOutside } from '@vueuse/components'
import menus from '../data/menu.js'

const showMenu = ref(false);

const menuActive = ref('');
const childActive = ref('');
const grandsonActive = ref('');

const closeMenu = () => {
  showMenu.value = false
}

const clickMenu = (key) => {
  menuActive.value = key
}

const clickChild = (key) => {
  childActive.value = key
}

const clickGrandson = (key) => {
  grandsonActive.value = key
}
</script>

<template>
  <button class="toggler" @click="showMenu = true">
    <img src="menu.svg" alt="">
  </button>

  <nav class="sidebar" :class="{ show: showMenu }" v-on-click-outside="closeMenu">
    <ul class="menu">
      <li v-for="menu in menus" :key="menu.key" :class="{ active: menuActive == menu.key }">
        <a @click="clickMenu(menu.key)">{{ menu.text }}</a>

        <ul v-if="menuActive == menu.key" class="children">
          <li v-for="child in menu.children" :key="child.key" :class="{ active: childActive == child.key }">
            <a @click="clickChild(child.key)">{{ child.text }}</a>

            <ul v-if="childActive == child.key" class="children">
              <li v-for="grandson in child.children" :key="grandson.key" :class="{ active: grandsonActive == grandson.key }">
                <a @click="clickGrandson(grandson.key)">{{ grandson.text }}</a>
              </li>
            </ul>

          </li>
        </ul>

      </li>
    </ul>
  </nav>
</template>

<style scoped lang="scss">
.toggler{
  display: flex;
  margin-left: auto;
  cursor: pointer;
}
.sidebar{
  position: absolute;
  top: 0;
  right: -50%;
  z-index: 1000;
  width: 50%;
  height: 100vh;
  padding: 1rem 0.75rem;
  padding-right: 0;
  background-color: rgba(0,0,0, 0.9);
  overflow-y: auto;
  transition: right 0.3s, opacity 0.3s;
  &.show{
    right: 0%;
  }
}
.menu{
  list-style: none;
  padding: 0;
  >li{
    padding: 0.375rem 0;
  }
  li{
    padding-left: 1.125rem;
    &.active{
      background-color: #666;
      >a{
        color: yellow;
      }
    }
    a{
      display: block;
      padding: 0.375rem 0;
      font-size: 18px;
      color: #fff;
      text-decoration: none;
      cursor: pointer;
    }
    .children{
      list-style: none;
      padding: 0;
    }
  }
}
</style>
