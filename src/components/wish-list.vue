<template>
  <div>
    <h1>My Holiday Gift Wishlist</h1>
    <input type="text" v-model="filterText" />
    <ul :class="$style.wishlist">
      <li :key="gift.id" v-for="gift in filteredWishlist">
        <h2>{{ gift.label }}</h2>
        <img :src="`/images/${gift.image}`" :alt="gift.label" />
        <p>{{ gift.wishes }}</p>
        <button @click="addWish(gift.id)">wish for that</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { reactive, toRefs, computed } from "vue";
import gifts from "../../public/data.json";

// TODO actually use the serverless functions to load this

export default {
  setup: () => {
    const state = reactive({
      giftList: gifts,
      filterText: "",
      filteredWishlist: computed(() => {
        return state.giftList.filter(gift => {
          const label = gift.label.toLowerCase();
          const search = state.filterText.toLowerCase();

          return label.includes(search);
        });
      })
    });

    state.giftList.forEach(gift => {
      gift.wishes = 0;
    });

    function addWish(id) {
      const gift = state.giftList.find(gift => id === gift.id);

      gift.wishes += 1;
    }

    return {
      ...toRefs(state),
      addWish
    };
  }
};
</script>

<style lang="scss" module>
.wishlist {
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(auto-fit, 150px);
  grid-template-rows: auto;
  list-style: none;
  justify-content: center;
  padding: 0;

  h2 {
    font-size: 1rem;
  }

  img {
    width: 100%;
  }
}
</style>
