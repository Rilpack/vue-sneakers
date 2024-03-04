<script setup>
import ReturnIcon from '@/components/Icons/ReturnIcon.vue'
import CartItemList from '@/components/CartItemList.vue'
import SpinnerIcon from '@/components/Icons/SpinnerIcon.vue'
import { ref, inject, computed } from 'vue'
import InfoBlock from './InfoBlock.vue'
import axios from 'axios'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})

const { carts, closeDrawer } = inject('carts')

const isCreating = ref(false)
const orderId = ref(null)

const cartIsEmpty = computed(() => carts.value.length === 0)

const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post('https://8f52475f6c5eaaba.mokky.dev/orders', {
      items: carts.value,
      totalPrice: props.totalPrice + props.vatPrice
    })
    carts.value = []
    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black opacity-60 z-10"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <div class="flex items-center gap-5 mb-5">
      <div @click="closeDrawer">
        <ReturnIcon />
      </div>
      <h2 class="text-2xl font-bold">Корзина</h2>
    </div>

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен!"
        :description="`Ваш заказ №${orderId} будет скоро передан курьерской доставке`"
        image-url="/order-success-icon.png"
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
          :disabled="buttonDisabled"
          @click="createOrder"
          class="flex items-center justify-center mt-4 bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 transition cursor-pointer"
        >
          <p v-if="!isCreating">Оформить заказ</p>
          <SpinnerIcon v-if="isCreating" />
        </button>
      </div>
    </div>
  </div>
</template>
