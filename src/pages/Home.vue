<script setup>
import CardList from '../components/CardList.vue'
import axios from 'axios'
import debounce from 'lodash.debounce'
import { inject, reactive, watch, ref, onMounted } from 'vue'

const items = ref([])

const addToCart = inject('addToCart')
const removeFromCart = inject('removeFromCart')
const cartItems = inject('cartItems')

const filters = reactive({
  searchQuery: '',
  sortBy: ''
})

const handleClickPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 500)

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
      searchQuery: filters.searchQuery
    }
    const { data } = await axios.get(
      `https://c106f40a73ac218b.mokky.dev/items?title=*${params.searchQuery}*&sortBy=${params.sortBy}`
    )
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false,
      favoriteId: null
    }))
  } catch (error) {
    console.log(error)
  }
}

const fetchItemsFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://c106f40a73ac218b.mokky.dev/favorites`)
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (error) {
    console.log(error)
  }
}

const onclickFavorite = async (item) => {
  try {
    if (!item.isFavorite) {

       const obj = {
        item_id: item.id,
       }

      item.isFavorite = true

      const { data } = await axios.post(`https://c106f40a73ac218b.mokky.dev/favorites`, obj)
      
      item.favoriteId = data.id
      console.log(data)
    } else {
      await axios.delete(`https://c106f40a73ac218b.mokky.dev/favorites/${item.favoriteId}`)
      item.isFavorite = false
      item.isFavoriteId = null
    }
  } catch (error) {
    console.log(error)
  }
}

onMounted(async () => {
  cartItems.value = JSON.parse(localStorage.getItem('cart')) || []

  await fetchItems()
  await fetchItemsFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(cartItems, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(filters, fetchItems)
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

    <div class="flex gap-4">
      <select
        @change="onChangeSelect"
        name=""
        id=""
        class="py-2 px-3 border rounded-md outline-none"
      >
        <option value="name">По названию</option>
        <option value="price">По цене (возрастанию)</option>
        <option value="-price">По цене (убыванию)</option>
      </select>

      <div class="relative">
        <img src="/search.svg" alt="search" class="absolute left-4 top-3" />
        <input
          @input="onChangeInput"
          type="text"
          placeholder="Поиск..."
          class="pl-11 pr-4 py-2 rounded-md border outline-none focus:border-gray-400"
        />
      </div>
    </div>
  </div>
  <CardList :items="items" @onclickFavorite="onclickFavorite" @handleClickPlus="handleClickPlus" />
</template>
