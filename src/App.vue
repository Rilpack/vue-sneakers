<script setup>
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import Drawer from './components/Drawer.vue'
import { onMounted, ref, reactive, watch } from 'vue'
import axios from 'axios'

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearch = (event) => {
  filters.searchQuery = event.target.value
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
    items.value = data
  } catch (err) {
    console.log(err)
  }
}

onMounted(fetchItems)
watch(filters, fetchItems)
</script>

<template>
  <!-- <Drawer /> -->
  <div class="w-4/5 mx-auto bg-white rounded-xl shadow-xl mt-14">
    <Header />

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

      <CardList :items="items" />
    </div>
  </div>
</template>
