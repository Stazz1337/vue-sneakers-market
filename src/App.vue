<script setup>
import { provide, ref, watch, computed } from 'vue'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

const drawerOpen = ref(false)
const cartItems = ref([])

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round(totalPrice.value * 0.05))

const addToCart = (item) => {
  cartItems.value.push(item)
  item.isAdded = true
  console.log(cartItems.value)
}

const removeFromCart = (item) => {
  console.log(item)
  cartItems.value = cartItems.value.filter((cartItem) => cartItem.id !== item.id)
  item.isAdded = false
}

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

watch(
  cartItems,
  () => {
    localStorage.setItem('cart', JSON.stringify(cartItems.value))
  },
  {
    deep: true
  }
)

provide('closeDrawer', closeDrawer)
provide('cartItems', cartItems)
provide('removeFromCart', removeFromCart)
provide('addToCart', addToCart)
provide('totalPrice', totalPrice)
</script>

<template>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Drawer v-if="drawerOpen" :vatPrice="vatPrice" />
    <Header @openDrawer="openDrawer" :totalPrice="totalPrice" />
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>
