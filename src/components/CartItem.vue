<script setup>
defineProps({
  imageUrl: String
})
</script>

<template>
  <div class="border border-slate-100 rounded-xl p-4 flex items-center gap-4">
    <img src="/sneakers/sneakers-1.jpg" alt="Sneaker" class="w-16 h-16" />
    <div class="flex flex-col">
      <p>Кроссовки Кроссовки Кроссовки Кроссовки</p>
      <div class="flex justify-between mt-2">
        <b>1200 руб.</b>
        <img class="cursor-pointer opacity-30 hover:opacity-100 transition" src="/close.svg" alt="close" />
      </div>
    </div>
  </div>
</template>
