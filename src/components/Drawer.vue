<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'
import { ref, inject, computed } from 'vue'
import axios from 'axios'

defineProps({
  vatPrice: Number
})

const cartItems = inject('cartItems')
const closeDrawer = inject('closeDrawer')
const totalPrice = inject('totalPrice')

const isCreateOrderLoading = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreateOrderLoading.value = true
    const { data } = await axios.post('https://c106f40a73ac218b.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: totalPrice.value
    })
    cartItems.value = []
    orderId.value = data.id
    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreateOrderLoading.value = false
  }
}

const isCartButtonDisabled = computed(
  () => isCreateOrderLoading.value || cartItems.value.length === 0
)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black bg-opacity-70 z-10"></div>
  <div class="bg-white w-96 h-full fixed top-0 right-0 z-20 p-8 flex flex-col">
    <DrawerHead @closeDrawer="closeDrawer" />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock v-if="orderId"
        title="Заказ оформлен"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской службе`"
        imageUrl="/order-success-icon.png"
      />
      <InfoBlock v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        imageUrl="/package-icon.png"
      />
    </div>


    <div v-else>
      <CartItemList />

      <div class="flex flex-col gap-4 my-7 mt-auto">
        <div class="flex gap-2">
          <p>Итого:</p>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} ₽</b>
        </div>
        <div class="flex gap-2">
          <p>Налог 5%:</p>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} ₽</b>
        </div>

        <button
          @click="createOrder"
          :disabled="isCartButtonDisabled"
          class="mt-7 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
