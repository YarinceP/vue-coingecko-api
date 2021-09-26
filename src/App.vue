<template>
  <div class="container">
    <div class="row">

      <input type="text" class="form-control bg-dark text-light rounded-0 border-0 my-4" v-model="textSearch"
             @keyup="searchCoin()" placeholder="Search Coin">

      <table class="table table-dark">
        <thead>
        <tr>
          <th v-for="title in titles" :key="title" class="text-muted"> {{ title }}</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
          <td>{{ index + 1 }}</td>
          <td>
            <img :src="coin.image" style="width: 2rem" class="me-2">
            <span>
              {{ coin.name }}
            </span>
            <span class="ms-2 text-uppercase text-muted">
              {{ coin.symbol }}
            </span>
          </td>
          <td>${{ coin.current_price }}</td>
          <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success': 'text-danger']">
            {{ coin.price_change_percentage_24h }}%
          </td>
          <td>${{ coin.total_volume }}</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: [
        '#',
        'Coin',
        'Price',
        'Price Chance',
        '24hn Volume',
      ],
      textSearch: ''
    }
  },
  async mounted() {
    const response = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data = await response.json()
    console.log(data);
    this.coins = data
    this.filteredCoins = data
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(coin =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase()));
    }
  }

}
</script>

<style>

</style>
