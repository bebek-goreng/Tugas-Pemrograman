<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="light">
        <ion-title>Crypto Prices</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">
      <div style="display: flex; justify-content: center; margin-bottom: 20px;">
        <ion-button @click="getData" color="primary">Get Data</ion-button>
      </div>

      <ion-list v-if="cryptos.length" class="crypto-table">
        <ion-item class="header-row">
          <ion-label class="header-label">Name</ion-label>
          <ion-label class="header-label">Symbol</ion-label>
          <ion-label class="header-label">Price (USD)</ion-label>
        </ion-item>
        <ion-item v-for="(crypto, index) in cryptos" :key="index" class="data-row">
          <ion-label>{{ crypto.name }}</ion-label>
          <ion-label>{{ crypto.symbol }}</ion-label>
          <ion-label>{{ crypto.price_usd }}</ion-label>
        </ion-item>
      </ion-list>

      <ion-spinner v-if="loading"></ion-spinner>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';

interface Crypto {
  name: string;
  symbol: string;
  price_usd: string;
}

export default defineComponent({
  name: 'HomePage',
  data() {
    return {
      cryptos: [] as Crypto[],
      loading: false,
    };
  },
  methods: {
    async getData() {
      this.loading = true;
      try {
        const response = await axios.get<{ data: Crypto[] }>('https://api.coinlore.net/api/tickers/');
        this.cryptos = response.data.data;
      } catch (error) {
        console.error('Error fetching data:', error);
      } finally {
        this.loading = false;
      }
    },
  },
});
</script>

<style scoped>
ion-button {
  margin-top: 1rem;
}

ion-spinner {
  margin-top: 1rem;
}

.crypto-table {
  background-color: black;
  color: white; 
  border: 1px solid #ccc;
  border-radius: 10px;
  overflow: hidden;
}

.header-row {
  background-color: #333; 
  border-bottom: 1px solid #ccc;
}

.header-label {
  font-weight: bold;
}

.data-row {
  border-bottom: 1px solid #ccc;
}

ion-label {
  width: 33.33%;
  text-align: center;
  padding: 10px 0;
  display: inline-block;
}
</style>
