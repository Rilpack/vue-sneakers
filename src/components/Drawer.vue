<script setup>
import ReturnIcon from '@/components/Icons/ReturnIcon.vue'
import CartItemList from '@/components/CartItemList.vue'
import { computed } from 'vue'
import InfoBlock from './InfoBlock.vue'

const emit = defineEmits(['closeDrawer', 'createOrder'])
const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
  disabledButton: Boolean
})
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black opacity-60 z-10"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <div class="flex items-center gap-5 mb-5">
      <div @click="() => emit('closeDrawer')">
        <ReturnIcon />
      </div>
      <h2 class="text-2xl font-bold">Корзина</h2>
    </div>

    <div v-if="!totalPrice" class="flex h-full items-center">
      <InfoBlock
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
    </div>

    <div v-else>
      <CartItemList />

      <div v-if="totalPrice" class="flex flex-col gap-4 mb-6 mt-7">
        <div class="flex gap-1">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed" />
          <b>{{ vatPrice }} ₽</b>
        </div>
        <div class="flex gap-1">
          <b>Итого:</b>
          <div class="flex-1 border-b border-dashed" />
          <b>{{ totalPrice + vatPrice }} ₽</b>
        </div>
        <button
          :disabled="disabledButton"
          @click="() => emit('createOrder')"
          class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 transition cursor-pointer"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
