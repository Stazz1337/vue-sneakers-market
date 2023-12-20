<script setup>
const emit = defineEmits(['openDrawer'])

defineProps({
  totalPrice: Number
})
</script>

<template>
  <header class="flex justify-between border-b border-slate-300 px-10 py-8">
    <div class="flex items-center gap-4">
      <img src="/logo.png" alt="Logo" class="w-10" />
      <div>
        <h2 class="text-xl font-bold uppercase">Vue sneakers</h2>
        <p class="text-slate-400">Магазин лучших кросовок</p>
      </div>
    </div>

    <ul class="flex gap-10 items-center">
      <li
        @click="emit('openDrawer')"
        class="flex gap-3 items-center text-gray-500 cursor-pointer hover:text-black"
      >
        <img src="/cart.svg" alt="Cart" />
        <b>{{totalPrice}} руб.</b>
      </li>
      <li class="flex gap-3 items-center text-gray-500 cursor-pointer hover:text-black">
        <img src="/heart.svg" alt="Favorite" />
        <p>Закладки</p>
      </li>
      <li class="flex gap-3 items-center text-gray-500 cursor-pointer hover:text-black">
        <img src="/profile.svg" alt="Profile" />
        <p>Профиль</p>
      </li>
    </ul>
  </header>
</template>
