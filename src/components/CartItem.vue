<script setup>
import CloseIconVue from './Icons/CloseIcon.vue'

defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number
})

const emit = defineEmits(['addToCart'])
</script>

<template>
  <div
    class="flex items-center justify-center border border-slate-100 p-4 rounded-xl gap-4 dark:border-slate-500"
  >
    <img class="w-16 h-16 rounded-xl border border-slate-500" :src="imageUrl" alt="sneaker" />

    <div class="flex flex-col flex-1">
      <p class="dark:text-slate-100">{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1 dark:text-slate-100">{{ price }} руб.</b>

        <div
          @click="() => emit('addToCart')"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
        >
          <CloseIconVue />
        </div>
      </div>
    </div>
  </div>
</template>
