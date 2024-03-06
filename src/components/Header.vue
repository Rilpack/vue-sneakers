<script setup>
import ProfileIconVue from './Icons/ProfileIcon.vue'
import HeartIconVue from './Icons/HeartIcon.vue'
import MoonIconVue from './Icons/MoonIcon.vue'
import CartIconVue from './Icons/CartIcon.vue'

const emit = defineEmits(['openDrawer'])
defineProps({
  totalPrice: Number
})

// Логика переключения темы
const toggleTheme = () => {
  const html = document.querySelector('html')
  html.classList.toggle('dark')
}
</script>

<template>
  <header class="flex justify-between border-b border-slate-300 px-8 py-4 dark:border-slate-600">
    <router-link to="/">
      <div class="flex items-center gap-4 select-none">
        <img src="/logo.png" alt="logo" class="w-10" />
        <div>
          <h2 class="text-xl font-bold uppercase dark:text-white">Vue Sneakers</h2>
          <p class="text-slate-400 dark:text-slate-200">Магазин лучших кроссовок</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-6">
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center gap-2 text-gray-600 hover:text-black cursor-pointer transition dark:text-slate-200"
      >
        <CartIconVue />
        <span class="">{{ totalPrice }} руб.</span>
      </li>
      <router-link to="/favorites">
        <li
          class="flex items-center gap-2 text-slate-600 hover:text-black cursor-pointer transition dark:text-slate-200"
        >
          <HeartIconVue />
          <span>Закладки</span>
        </li>
      </router-link>
      <li
        class="flex items-center gap-2 text-slate-600 hover:text-black cursor-pointer transition dark:text-slate-200"
      >
        <ProfileIconVue />
        <span>Профиль</span>
      </li>
      <li class="cursor-pointer ml-8" @click="toggleTheme">
        <MoonIconVue />
      </li>
    </ul>
  </header>
</template>
