<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const cartItems = inject('cartItems')
</script>

<template>
  <div class="flex flex-col gap-4">
    <CartItem
      v-for="item in cartItems"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :price="item.price"
      :imageUrl="item.imageUrl"
    />
  </div>
</template>
