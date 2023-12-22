<script setup>
import DrawerHead from "./DrawerHead.vue";
import CartItemList from "./CartItemList.vue";
import InfoBlock from "./InfoBlock.vue";
import { computed, inject } from "vue";

const emit = defineEmits(["createOrder"]);

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
  isCreatingOrder: Boolean,
});

const { closeDrawer } = inject("cart");

const buttonDisabled = computed(() => {
  return !props.totalPrice || props.isCreatingOrder;
});  

</script>

<template>
  <div
    @click="closeDrawer"
    class="fixed top-0 left-0 right-0 bottom-0 bg-black opacity-60 z-10"
  ></div>
  <div
    class="bg-white w-1/3 z-20 fixed right-0 top-0 bottom-0 p-8 flex flex-col"
  >
    <DrawerHead />
    <InfoBlock title="Корзина пустая" description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ" image-url="/package-icon.png" />
    <CartItemList />
    <div class="flex flex-col gap-4 mb-7 mt-auto">
      <div class="flex justify-between items-end gap-1">
        <span class="whitespace-nowrap">Итого:</span>
        <div class="border-b border-dashed w-full"></div>
        <span class="whitespace-nowrap"
          ><b>{{ totalPrice }} $</b></span
        >
      </div>
      <div class="flex justify-between items-end gap-1">
        <span class="whitespace-nowrap">Налог 5%:</span>
        <div class="border-b border-dashed w-full"></div>
        <span class="whitespace-nowrap"
          ><b>{{ vatPrice }} $</b></span
        >
      </div>
    </div>

    <button
      @click="() => emit('createOrder')"
      :disabled="buttonDisabled"
      class="bg-lime-500 w-full rounded-xl px-1 py-3 text-white transition-colors cursor-pointer hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-300"
    >
      Оформить заказ
    </button>
  </div>
</template>
