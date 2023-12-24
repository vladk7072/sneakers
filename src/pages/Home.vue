<script setup>
import axios from "axios";
import CardList from "../components/CardList.vue";
import debounce from "lodash.debounce";
import { ref, reactive, computed, inject, watch, onMounted } from "vue";

const { cart, addToCart, removeFromCart } = inject("cart");

const items = ref([]);

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

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value;
};
const onChangeSearchInput = debounce((e) => {
  filters.searchQuery = e.target.value;
}, 500);

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id,
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

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://1d8397323be8df4a.mokky.dev/favorites`
    );
    items.value = items.value.map((item) => {
      const favorite = favorites.find(
        (favorite) => favorite.item_id === item.id
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
  const localCart = localStorage.getItem("cart");
  if (!localCart) {
    localStorage.setItem("cart", JSON.stringify([]));
  } else {
    cart.value = localCart ? JSON.parse(localCart) : [];
  }

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
  }));
});
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }));
});
watch(filters, fetchItems);
</script>

<template>
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
</template>
