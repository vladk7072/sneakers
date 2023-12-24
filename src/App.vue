<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from "vue";
import Header from "./components/Header.vue";
import Drawer from "./components/Drawer.vue";
import axios from "axios";

/* Start  Cart */
const cart = ref([]);
const isCreatingOrder = ref(false);

const drawerOpen = ref(false);

const closeDrawer = () => {
  drawerOpen.value = false;
};

const openDrawer = () => {
  drawerOpen.value = true;
};

const totalPrice = computed(() => {
  return cart.value.reduce((acc, item) => acc + item.price, 0);
});
const vatPrice = computed(() => {
  return Math.round((totalPrice.value * 5) / 100);
});

const createOrder = async () => {
  try {
    isCreatingOrder.value = true;
    const { data } = await axios.post(
      `https://1d8397323be8df4a.mokky.dev/orders`,
      {
        items: cart.value,
        totalPrice: totalPrice.value,
      }
    );
    cart.value = [];
    return data;
  } catch (err) {
    isCreatingOrder.value = false;
    console.log(err);
  } finally {
    isCreatingOrder.value = false;
  }
};

const addToCart = (item) => {
  cart.value.push(item);
  item.isAdded = true;
};

const removeFromCart = (item) => {
  cart.value.splice(
    cart.value.findIndex((el) => el.id === item.id),
    1
  );
  item.isAdded = false;
};

watch(
  cart,
  () => {
    localStorage.setItem("cart", JSON.stringify(cart.value));
  },
  {
    deep: true,
  }
);

provide("cart", {
  cart,
  openDrawer,
  closeDrawer,
  addToCart,
  removeFromCart,
});

/* End Cart */
</script>

<template>
  <Drawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    @create-order="createOrder"
    :is-creating-order="isCreatingOrder"
  />
  <main class="w-4/5 mx-auto bg-white rounded-xl shadow-xl my-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <div class="p-10">
      <router-view></router-view>
      <Home />
    </div>
  </main>
</template>
