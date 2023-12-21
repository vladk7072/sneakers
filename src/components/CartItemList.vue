<script setup>
import { inject } from "vue";
import CartItem from "./CartItem.vue";

const { cart, removeFromCart } = inject("cart");

</script>

<template>
  <div class="my-4 pr-3 flex flex-col gap-3 overflow-y-auto">
    <CartItem v-for="item in cart" :key="item.id" :item="item" @on-click-remove="() => removeFromCart(item)" />
  </div>
</template>
