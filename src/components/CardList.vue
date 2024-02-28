<script setup>
import Card from '@/components/Card.vue'

defineProps({
  items: Array
})

const onClickAdd = () => {
  alert('asdasdasdsad')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5 mt-10">
    <Card
      v-for="item in items"
      :key="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :isFavorite="true"
      :isAdded="false"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
