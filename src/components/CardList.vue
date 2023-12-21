<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
  isFavorites: Boolean
})

const emit = defineEmits(['onclickFavorite', 'handleClickPlus'])
</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isAdded="item.isAdded"
      :isFavorite="item.isFavorite"
      :onclickAddToCart="isFavorites ? null : () => emit('handleClickPlus', item)"
      :onclickFavorite="isFavorites ? null : () => emit('onclickFavorite', item)"
    />
  </div>
</template>
