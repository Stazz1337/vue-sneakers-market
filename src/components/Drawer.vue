<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black bg-opacity-70 z-10"></div>
  <div class="bg-white w-96 h-full fixed top-0 right-0 z-20 p-8 flex flex-col">
    <DrawerHead />
    <CartItemList />

    <div class="flex flex-col gap-4 my-7 mt-auto">
      <div class="flex gap-2">
        <p>Итого:</p>
        <div class="flex-1 border-b border-dashed"></div>
        <b>1200 руб.</b>
      </div>
      <div class="flex gap-2">
        <p>Налог 5%:</p>
        <div class="flex-1 border-b border-dashed"></div>
        <b>900 руб.</b>
      </div>

      <button
        disabled=""
        class=" mt-7 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
