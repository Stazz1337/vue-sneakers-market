<script setup>
import axios from 'axios'

import { onMounted, ref, inject } from 'vue'
import CardList from '../components/CardList.vue'

const favorites = ref([])

const addToCart = inject('addToCart')
const removeFromCart = inject('removeFromCart')

const handleClickPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://c106f40a73ac218b.mokky.dev/favorites?_relations=items'
    )

    favorites.value = data.map((item) => item.item)
    console.log(favorites.value)
  } catch (error) {
    console.log(error)
  }
})


</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>
  <CardList :items="favorites" @handleClickPlus="handleClickPlus" :isFavorites="true" />
</template>
