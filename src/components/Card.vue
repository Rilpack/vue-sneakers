<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})
</script>

<template>
  <div
    class="flex flex-col items-center bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition dark:bg-slate-800 dark:border-slate-600"
  >
    <div class="relative">
      <img
        class="w-full h-auto max-w-[266px] rounded-3xl border border-slate-700"
        :src="imageUrl"
        alt="sneaker"
      />
      <img
        v-if="onClickFavorite"
        @click="onClickFavorite"
        class="absolute top-4 left-4 border border-slate-200 rounded-xl"
        :src="isFavorite ? '/like-2.svg' : '/like-1.svg'"
        alt="like"
      />
    </div>

    <p class="w-full mt-6 dark:text-white text-lg">{{ title }}</p>

    <div class="flex w-full justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400 dark:text-slate-100">Цена:</span>
        <b class="dark:text-slate-400">{{ price }} руб.</b>
      </div>
      <img
        v-if="onClickAdd"
        @click="onClickAdd"
        :src="isAdded ? '/checked.svg' : '/plus.svg'"
        alt="plus"
      />
    </div>
  </div>
</template>
