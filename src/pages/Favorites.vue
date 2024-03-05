<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

import CardList from '../components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://8f52475f6c5eaaba.mokky.dev/favorites?_relations=items'
    )

    console.log(data)
    favorites.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-8 dark:text-slate-100">Мои закладки</h2>

  <CardList :items="favorites" :isFavorite="true" />
</template>
