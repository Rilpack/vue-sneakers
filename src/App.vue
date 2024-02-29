<script setup>
import { onMounted, ref, reactive, watch, provide, computed } from 'vue'
import axios from 'axios'
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])
const carts = ref([])
const drawerOpen = ref(false)
const totalPrice = computed(() => carts.value.reduce((total, cart) => total + cart.price, 0))

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const addToCart = (item) => {
  if (!item.isAdded) {
    carts.value.push(item)
    item.isAdded = true
  } else {
    carts.value.splice(carts.value.indexOf(item), 1)
    item.isAdded = false
  }
  console.log(carts.value)
}

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearch = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://8f52475f6c5eaaba.mokky.dev/favorites`)

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)
      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {}
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

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = { parentId: item.id }
      item.isFavorite = true
      const { data } = await axios.post(`https://8f52475f6c5eaaba.mokky.dev/favorites`, obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://8f52475f6c5eaaba.mokky.dev/favorites/${item.favoriteId}`)
      delete item.favoriteId
    }
  } catch (err) {}
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

watch(filters, fetchItems)

provide('carts', { carts, addToCart })
</script>

<template>
  <Drawer :total-price="totalPrice" @close-drawer="closeDrawer" v-if="drawerOpen" />
  <div class="w-4/5 mx-auto bg-white rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />

    <div class="p-10">
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
    </div>
  </div>
</template>
