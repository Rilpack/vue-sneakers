<script setup>
import { ref, watch, provide, computed } from 'vue'
import Header from '@/components/Header.vue'
import Drawer from './components/Drawer.vue'

/* Корзина */
const carts = ref([])

const drawerOpen = ref(false)

const totalPrice = computed(() => carts.value.reduce((total, cart) => total + cart.price, 0))
const vatPrice = computed(() => Math.round(totalPrice.value * 5) / 100)

const addToCart = (item) => {
  if (!item.isAdded) {
    carts.value.push(item)
    item.isAdded = true
  } else {
    carts.value.splice(carts.value.indexOf(item), 1)
    item.isAdded = false
  }
}

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

watch(
  carts,
  () => {
    localStorage.setItem('carts', JSON.stringify(carts.value))
  },
  {
    deep: true
  }
)

provide('carts', { carts, addToCart, closeDrawer })
/* Корзина */
</script>

<template>
  <Drawer :total-price="totalPrice" :vat-price="vatPrice" v-if="drawerOpen" />
  <div class="w-4/5 mx-auto bg-white rounded-xl shadow-xl mt-14 mb-14 dark:bg-slate-700">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />

    <div class="p-10">
      <router-view> </router-view>
    </div>
  </div>
</template>
