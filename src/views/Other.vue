<template>
  <div class="w-full flex flex-col items-center">
    <d-header />
    <h2 class="text-2xl text-center mt-16 mb-16 font-light">
      Other Currencies
    </h2>
    <p>Select the coin:</p>

    <ul class="flex flex-row flex-wrap justify-center">
      <li class="flex flex-col justify-center items-center m-7">
        <img
          class="w-20 h-20 mb-3 animate-pulse"
          :src="valueDacxi.image"
          :alt="valueDacxi.id"
        />
        <router-link
          class="font-semibold text-xl hover:text-orange"
          :to="{ name: 'Date', params: { id: 'dacxi' } }"
          >{{ valueDacxi.symbol }}</router-link
        >
        <span class="text-sm text-center text-green"
          >R$ {{ valueDacxi.current_price }} BRL</span
        >
      </li>

      <li
        class="flex flex-col justify-center items-center m-7"
        v-for="coin in valuesCoins"
        :key="coin.id"
      >
        <img
          class="w-20 h-20 mb-3 animate-pulse"
          :src="coin.image"
          :alt="coin.id"
        />
        <router-link
          class="font-semibold text-xl hover:text-orange transition duration-150"
          :to="{ name: 'Date', params: { id: coin.id } }"
          >{{ coin.symbol }}</router-link
        >
        <span class="text-sm text-center text-green"
          >R$ {{ coin.current_price }} BRL</span
        >
      </li>
    </ul>

    <router-link to="/" class="mx-auto mt-5">
      <d-button>home</d-button>
    </router-link>
    <d-footer class="mt-20" />
  </div>
</template>

<script>
import DButton from "../components/DButton.vue";
import DFooter from "../components/DFooter.vue";
import DHeader from "../components/DHeader.vue";
import Coins from "../services/coins";
export default {
  components: { DHeader, DFooter, DButton },
  data() {
    return {
      valuesCoins: [],
      valueDacxi: "",
    };
  },
  created() {
    this.getCoin();
    this.getDacxi();
  },
  mounted() {
    this.attCoin();
  },
  methods: {
    getCoin() {
      Coins.listOthers().then((response) => {
        let eth = response.data.filter((coin) => coin.id === "ethereum");
        let luna = response.data.filter((coin) => coin.id === "terra-luna");
        let atom = response.data.filter((coin) => coin.id === "cosmos");
        const newArray = [eth[0], luna[0], atom[0]];
        this.valuesCoins = newArray;
      });
    },
    getDacxi() {
      Coins.listDacxi().then((response) => {
        this.valueDacxi = response.data[0];
      });
    },
    attCoin() {
      setInterval(() => {
        this.getCoin();
        this.getDacxi();
      }, 10000);
    },
  },
  beforeDestroy() {
    clearInterval(this.attCoin);
  },
};
</script>
<style></style>
