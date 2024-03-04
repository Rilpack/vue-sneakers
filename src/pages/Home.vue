<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { reactive, inject, watch, ref, onMounted } from 'vue'
import CardList from '@/components/CardList.vue'
import debounce from 'lodash.debounce'
import axios from 'axios'

const { carts, addToCart } = inject('carts')
const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearch = debounce((event) => {
  filters.searchQuery = event.target.value
}, 500)

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = { item_id: item.id }
      item.isFavorite = true
      const { data } = await axios.post(`https://8f52475f6c5eaaba.mokky.dev/favorites`, obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://8f52475f6c5eaaba.mokky.dev/favorites/${item.favoriteId}`)
      delete item.favoriteId
    }
  } catch (err) {
    undefined
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://8f52475f6c5eaaba.mokky.dev/favorites`)

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)
      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    undefined
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(`https://8f52475f6c5eaaba.mokky.dev/items`, {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

watch(filters, fetchItems)
watch(carts, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

onMounted(async () => {
  const localCart = localStorage.getItem('carts')
  carts.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: carts.value.some((cartItem) => cartItem.id === item.id)
  }))
})
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

    <div class="flex gap-4">
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
        <option value="name">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>

      <div class="relative">
        <img class="absolute top-3 left-3" src="/search.svg" />
        <input
          @input="onChangeSearch"
          class="border rounded-md py-2 pl-9 pr-4 outline-none focus:border-gray-400"
          placeholder="Поиск"
        />
      </div>
    </div>
  </div>

  <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="addToCart" />
</template>
