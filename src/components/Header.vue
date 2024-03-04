<script setup>
const emit = defineEmits(['openDrawer'])
defineProps({
  totalPrice: Number
})
</script>

<template>
  <header class="flex justify-between border-b border-slate-300 px-8 py-4">
    <router-link to="/">
      <div class="flex items-center gap-4 select-none">
        <img src="/logo.png" alt="logo" class="w-10" />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-400">Магазин лучших кроссовок</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-6">
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center gap-2 text-gray-600 hover:text-black cursor-pointer transition"
      >
        <img src="/cart.svg" alt="cart" />
        <span class="">{{ totalPrice }} руб.</span>
      </li>
      <router-link to="/favorites">
        <li
          class="flex items-center gap-2 text-slate-600 hover:text-black cursor-pointer transition"
        >
          <img src="/heart.svg" alt="cart" />
          <span>Закладки</span>
        </li>
      </router-link>
      <li class="flex items-center gap-2 text-slate-600 hover:text-black cursor-pointer transition">
        <img src="/profile.svg" alt="cart" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>
