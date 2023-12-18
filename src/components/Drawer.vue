<script setup>
import DrawerHead from "./DrawerHead.vue";
import CartItemList from "./CartItemList.vue";
</script>

<template>
  <div
    class="fixed top-0 left-0 right-0 bottom-0 bg-black opacity-60 z-10"
  ></div>
  <div class="bg-white w-1/3 z-20 fixed right-0 top-0 bottom-0 p-8 flex flex-col">
    <DrawerHead />
    <CartItemList />

    <div class="flex flex-col gap-4 mb-7 mt-auto">
      <div class="flex justify-between items-end gap-1">
        <span class="whitespace-nowrap">Итого:</span>
        <div class="border-b border-dashed w-full"></div>
        <span class="whitespace-nowrap"><b>2135 uan</b></span>
      </div>
      <div class="flex justify-between items-end gap-1">
        <span class="whitespace-nowrap">Налог 5%:</span>
        <div class="border-b border-dashed w-full"></div>
        <span class="whitespace-nowrap"><b>14 uan</b></span>
      </div>
    </div>

    <button
      disabled
      class="bg-lime-500 w-full rounded-xl px-1 py-3 text-white transition-colors cursor-pointer hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-300"
    >
      Оформить заказ
    </button>
  </div>
</template>
