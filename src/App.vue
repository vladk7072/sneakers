<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import Header from "./components/Header.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";
import axios from "axios";

const items = ref([]);
const filters = reactive({
  sortBy: "",
  searchQuery: "",
});

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value;
};

onMounted(async () => {
  try {
    const { data } = await axios.get(
      "https://1d8397323be8df4a.mokky.dev/items"
    );
    items.value = data;
  } catch (err) {
    console.log(err);
  }
});

watch(
  () => filters.sortBy,
  async () => {
    try {
      const { data } = await axios.get(
        "https://1d8397323be8df4a.mokky.dev/items?sortBy=" + filters.sortBy
      );
      items.value = data;
    } catch (err) {
      console.log(err);
    }
  }
);
</script>

<template>
  <!-- <Drawer /> -->
  <main class="w-4/5 mx-auto bg-white rounded-xl shadow-xl my-14">
    <Header />
    <div class="p-10">
      <div class="flex justify-between items-center mb-8">
        <h2 class="text-3xl font-bold mr-auto">Все кроссовки:</h2>
        <select
          @change="onChangeSelect"
          class="py-[10px] px-3 border rounded-md outline-none mr-4"
        >
          <option value="name">По названию</option>
          <option value="price">По цене (дешевые)</option>
          <option value="-price">По цене (дорогие)</option>
        </select>
        <div class="relative">
          <img
            class="absolute top-[50%] left-4 -translate-y-2/4"
            src="/search.svg"
            alt=""
          />
          <input
            class="border rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400"
            type="text"
          />
        </div>
      </div>
      <div class="mt-10">
        <CardList :items="items" />
      </div>
    </div>
  </main>
</template>
