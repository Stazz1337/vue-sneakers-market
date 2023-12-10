<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})

const onclickAddToCart = () => {}

const onclickFavorite = () => {}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isAddedToCart="true"
      :isFavorite="false"
      :onclickAddToCart="onclickAddToCart"
      :onclickFavorite="onclickFavorite"
    />
  </div>
</template>
