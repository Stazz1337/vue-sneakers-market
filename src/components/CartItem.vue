<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,

})
</script>

<template>
  <div class="border border-slate-100 rounded-xl p-4 flex items-center gap-4">
    <img :src="imageUrl" :alt="title" class="w-16 h-16" />
    <div class="flex flex-col flex-1">
      <p>{{title}}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{price}} руб.</b>
        <img class="cursor-pointer opacity-30 hover:opacity-100 transition" src="/close.svg" alt="close" />
      </div>
    </div>
  </div>
</template>
