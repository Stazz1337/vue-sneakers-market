<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const cartItems = inject('cartItems')
const removeFromCart = inject('removeFromCart')
</script>

<template>
  <div class="flex flex-col gap-4" v-auto-animate>
    <CartItem
      v-for="item in cartItems"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :price="item.price"
      :imageUrl="item.imageUrl"
      @removeFromCart="removeFromCart(item)"
    />
  </div>
</template>
