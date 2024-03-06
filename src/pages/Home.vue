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
}, 400)

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
watch(filters, fetchFavorites)
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
    <h2 class="text-3xl font-bold mb-8 dark:text-slate-100">Все кроссовки</h2>

    <div class="flex gap-4">
      <div class="relative">
        <select
          placeholder="Фильтр"
          @change="onChangeSelect"
          class="bg-transparent border-2 rounded-md appearance-none focus-visible:ring-gray-400 focus:border-gray-400 hover:border-gray-400 block w-full p-2 pl-4 w-48 dark:bg-[#0F1A26] dark:border dark:border-slate-700 dark:placeholder-slate-400 dark:text-white"
        >
          <option class="text-black dark:text-white" value="name">По названию</option>
          <option class="text-black dark:text-white" value="price">По цене (дешевые)</option>
          <option class="text-black dark:text-white" value="-price">По цене (дорогие)</option>
        </select>
        <div class="absolute right-0 top-0 bottom-0 flex items-center pointer-events-none pr-3">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-4 w-4 text-gray-500 dark:text-white"
            viewBox="0 0 20 20"
            fill="currentColor"
          >
            <path
              fill-rule="evenodd"
              d="M10 12a1 1 0 0 0 .707-.293l4-4a1 1 0 1 0-1.414-1.414L10 9.586 6.707 6.293a1 1 0 0 0-1.414 1.414l4 4a1 1 0 0 0 .707.293z"
              clip-rule="evenodd"
            />
          </svg>
        </div>
      </div>
      <div class="relative">
        <img class="absolute top-3 left-3" src="/search.svg" />
        <input
          @input="onChangeSearch"
          class="border-2 rounded-md py-2 pl-9 pr-4 outline-none focus:border-black hover:border-gray-400 bg-transparent dark:text-white dark:border dark:border-slate-700 dark:hover:border-white dark:focus:border-white"
          placeholder="Поиск"
        />
      </div>
    </div>
  </div>

  <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="addToCart" />
</template>
