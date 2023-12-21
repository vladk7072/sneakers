<script setup>
import { onMounted, provide, reactive, ref, watch } from "vue";
import Header from "./components/Header.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";
import axios from "axios";

const items = ref([]);
const cart = ref([]);
const drawerOpen = ref(false);

const closeDrawer = () => {
  drawerOpen.value = false;
};

const openDrawer = () => {
  drawerOpen.value = true;
};

const filters = reactive({
  sortBy: "title",
  searchQuery: "",
});

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removeFromCart(item);
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

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value;
};
const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value;
};

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://1d8397323be8df4a.mokky.dev/favorites`
    );
    items.value = items.value.map((item) => {
      const favorite = favorites.find(
        (favorite) => favorite.parentId === item.id
      );
      if (!favorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      };
    });
  } catch (err) {
    console.log(err);
  }
};

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
      };
      item.isFavorite = true;
      const { data } = await axios.post(
        `https://1d8397323be8df4a.mokky.dev/favorites`,
        obj
      );
      item.favoriteId = data.id;
    } else {
      item.isFavorite = false;
      await axios.delete(
        `https://1d8397323be8df4a.mokky.dev/favorites/${item.favoriteId}`
      );
      item.favoriteId = null;
    }
  } catch (err) {
    console.log(err);
  }
};

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    };

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }

    const { data } = await axios.get(
      `https://1d8397323be8df4a.mokky.dev/items`,
      {
        params,
      }
    );
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }));
  } catch (err) {
    console.log(err);
  }
};

onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
});
watch(filters, fetchItems);

provide("cart", {
  cart,
  openDrawer,
  closeDrawer,
  addToCart,
  removeFromCart,
});
</script>

<template>
  <Drawer v-if="drawerOpen" />
  <main class="w-4/5 mx-auto bg-white rounded-xl shadow-xl my-14">
    <Header @open-drawer="openDrawer" />
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
            @input="onChangeSearchInput"
            class="border rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400"
            type="text"
          />
        </div>
      </div>
      <div class="mt-10">
        <CardList
          :items="items"
          @add-to-favorite="addToFavorite"
          @on-click-add-plus="onClickAddPlus"
        />
      </div>
    </div>
  </main>
</template>
