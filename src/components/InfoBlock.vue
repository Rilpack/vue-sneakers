<script setup>
defineProps({
  imageUrl: String,
  title: String,
  description: String
})
</script>

<template>
  <div class="flex flex-col items-center text-center w-xs mx-auto">
    <img height="120" width="120" :src="imageUrl" alt="Info image" />
    <h2 class="mt-4 text-2xl font-medium dark:text-slate-100">{{ title }}</h2>
    <p class="text-gray-400 mt-2 dark:text-slate-400">{{ description }}</p>
  </div>
</template>
