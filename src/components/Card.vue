<script setup>
import LikeIconVue from './Icons/LikeIcon.vue'
import LikeIconActiveVue from './Icons/LikeIconActive.vue'
import PlusIconVue from './Icons/PlusIcon.vue'
import CheckedIconVue from './Icons/CheckedIcon.vue'

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
    class="flex flex-col items-center justify-between bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition dark:bg-[#1F2937] dark:border-slate-600 dark:shadow-slate-700"
  >
    <div class="relative">
      <img
        class="w-full h-auto max-w-[266px] rounded-3xl border border-slate-700"
        :src="imageUrl"
        alt="sneaker"
      />

      <div v-if="isFavorite" class="absolute top-4 left-4">
        <LikeIconActiveVue v-if="onClickFavorite" @click="onClickFavorite" />
      </div>

      <div v-if="!isFavorite" class="absolute top-4 left-4">
        <LikeIconVue v-if="onClickFavorite" @click="onClickFavorite" />
      </div>
    </div>

    <p class="w-full mt-6 dark:text-white text-lg">{{ title }}</p>

    <div class="flex w-full justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400 dark:text-slate-100">Цена:</span>
        <b class="dark:text-slate-400">{{ price }} руб.</b>
      </div>

      <div v-if="!isAdded">
        <PlusIconVue v-if="onClickAdd" @click="onClickAdd" />
      </div>

      <div v-if="isAdded">
        <CheckedIconVue v-if="onClickAdd" @click="onClickAdd" />
      </div>
    </div>
  </div>
</template>
