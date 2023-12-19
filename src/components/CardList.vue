<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})

const emit = defineEmits(['onclickFavorite', 'addToCart'])
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isAdded="item.isAdded"
      :isFavorite="item.isFavorite"
      :onclickAddToCart="() => emit('addToCart', item)"
      :onclickFavorite="() => emit('onclickFavorite', item)"
    />
  </div>
</template>
