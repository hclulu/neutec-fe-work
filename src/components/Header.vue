<script setup>
import { ref, watch } from 'vue'
import { vOnClickOutside } from '@vueuse/components'
import menu from '../data/menu.js'

const showMenu = ref(false)

const menuActive = ref({
  parent: JSON.parse(localStorage.getItem('menuActive'))?.parent,
  child: JSON.parse(localStorage.getItem('menuActive'))?.child,
  grandson: JSON.parse(localStorage.getItem('menuActive'))?.grandson,
});

const closeMenu = () => {
  showMenu.value = false
}

const clickMenu = (key, type) => {
  menuActive.value[type] = key
  saveMenu()
}

const saveMenu = () => {
  localStorage.setItem('menuActive', JSON.stringify(menuActive.value))
}

const menuSelected = ref('')

watch(menuSelected, (newMenuSelected) => {
  let split = newMenuSelected.split(',')
  menuActive.value.parent = split[0]
  menuActive.value.child = split[1]
  menuActive.value.grandson = split[2]
  saveMenu()
})
</script>

<template>
  <button class="toggler" @click="showMenu = true">
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#555" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-menu"><line x1="3" y1="12" x2="21" y2="12"></line><line x1="3" y1="6" x2="21" y2="6"></line><line x1="3" y1="18" x2="21" y2="18"></line></svg>
  </button>

  <nav class="sidebar" :class="{ show: showMenu }" v-on-click-outside="closeMenu">
    <ul class="menu">
      <!-- 第一層 -->
      <li v-for="parent in menu" :key="parent.key" :class="{ active: menuActive.parent == parent.key }">
        <a @click="clickMenu(parent.key, 'parent')">{{ parent.text }}</a>

        <!-- 第二層 -->
        <ul v-if="menuActive.parent == parent.key" class="children">
          <li v-for="child in parent.children" :key="child.key" :class="{ active: menuActive.child == child.key }">
            <a @click="clickMenu(child.key, 'child')">{{ child.text }}</a>

            <!-- 第三層 -->
            <ul v-if="menuActive.child == child.key" class="children">
              <li v-for="grandson in child.children" :key="grandson.key" :class="{ active: menuActive.grandson == grandson.key }">
                <a @click="clickMenu(grandson.key, 'grandson')">{{ grandson.text }}</a>
              </li>
            </ul>

          </li>
        </ul>

      </li>
    </ul>

    <hr>

    <select class="select" v-model="menuSelected">
      <option disabled value="">請選擇</option>
      
      <!-- 第一層 -->
      <template v-for="parent in menu" :key="parent.key">
        <option :value="parent.key">{{ parent.text }}</option>

        <!-- 第二層 -->
        <template v-for="child in parent.children" :key="child.key">
          <option :value="parent.key+','+child.key">{{ child.text }}</option>

          <!-- 第三層 -->
          <template v-for="grandson in child.children" :key="grandson.key">
            <option :value="parent.key+','+child.key+','+grandson.key">{{ grandson.text }}</option>
          </template>
        </template>
      </template>
    </select>
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
  background-color: rgba(0,0,0, 0.9);
  overflow-y: auto;
  transition: right 0.3s, opacity 0.3s;
  &.show{
    right: 0%;
  }
}
.menu{
  list-style: none;
  margin-right: -0.75rem;
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
.select{
  width: 100%;
  padding: 0.25rem;
  font-size: 18px;
  outline: 0;
}
hr{
  margin: 1rem 0;
  border-color: #000;
}
</style>
