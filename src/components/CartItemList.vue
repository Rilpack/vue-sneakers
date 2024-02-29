<script setup>
import { inject } from 'vue'
import CartItem from '@/components/CartItem.vue'

const { carts, addToCart } = inject('carts')
</script>

<template>
  <div class="flex flex-col flex-1 gap-4">
    <CartItem
      v-for="cart in carts"
      :key="cart.id"
      :id="cart.id"
      :imageUrl="cart.imageUrl"
      :title="cart.title"
      :price="cart.price"
      @add-to-cart="() => addToCart(cart)"
    />
  </div>
</template>
