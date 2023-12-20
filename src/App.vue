<script setup>
import { onMounted, provide, reactive, ref, watch, computed } from 'vue'
import axios from 'axios'
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])
const drawerOpen = ref(false)
const cartItems = ref([])

const isCreateOrderLoading = ref(false)

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

const handleClickPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const isCartButtonDisabled = computed(
  () => isCreateOrderLoading.value || cartItems.value.length === 0
)

const createOrder = async () => {
  try {
    isCreateOrderLoading.value = true
    const { data } = await axios.post('https://c106f40a73ac218b.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: totalPrice.value
    })
    cartItems.value = []
    console.log(data)
    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreateOrderLoading.value = false
  }
}

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const filters = reactive({
  searchQuery: '',
  sortBy: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchItemsFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://c106f40a73ac218b.mokky.dev/favorites`)
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

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

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
      searchQuery: filters.searchQuery
    }
    const { data } = await axios.get(
      `https://604781a0efa572c1.mokky.dev/items?title=*${params.searchQuery}*&sortBy=${params.sortBy}`
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

onMounted(async () => {
  cartItems.value = JSON.parse(localStorage.getItem('cart')) || []

 

  await fetchItems()
  await fetchItemsFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(filters, fetchItems)

watch(cartItems, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

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

const onclickFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const { data } = await axios.post(`https://c106f40a73ac218b.mokky.dev/favorites`, {
        parentId: item.id
      })
      item.isFavorite = true
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
</script>

<template>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Drawer
      v-if="drawerOpen"
      :vatPrice="vatPrice"
      :totalPrice="totalPrice"
      @createOrder="createOrder"
      :isCartButtonDisabled="isCartButtonDisabled"
    />
    <Header @openDrawer="openDrawer" :totalPrice="totalPrice" />
    <div class="p-10">
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
      <CardList
        :items="items"
        @onclickFavorite="onclickFavorite"
        @handleClickPlus="handleClickPlus"
      />
    </div>
  </div>
</template>

<style scoped></style>
