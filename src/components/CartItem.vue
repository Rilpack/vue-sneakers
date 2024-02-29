<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number
})

const emit = defineEmits(['addToCart'])
</script>

<template>
  <div class="flex items-center justify-center border border-slate-100 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" :src="imageUrl" alt="sneaker" />

    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }} руб.</b>
        <img
          @click="() => emit('addToCart')"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
          src="/close.svg"
          alt="delete-sneaker"
        />
      </div>
    </div>
  </div>
</template>
