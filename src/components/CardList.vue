<script setup>
import Card from '@/components/Card.vue'

const onClickAdd = () => {
  alert('asdasdasdsad')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      imageUrl="/sneakers/sneakers-1.jpg"
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      :price="12000"
      :isFavorite="true"
      :isAdded="false"
      :onClickAdd="onClickAdd"
    />
    <Card
      imageUrl="/sneakers/sneakers-2.jpg"
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      :price="2000"
      :isFavorite="false"
      :isAdded="true"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
