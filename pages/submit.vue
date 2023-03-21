<template>
  <div class="summary">
    <div v-for="(king, index) in kings" :key="index">
      <h2>{{ king.name }}</h2>
      <img :src="king.photo" alt="King photo" />
      <div class="info">
        <div class="label">Total coins:</div>
        <div class="value">{{ king.coins.length }}</div>
      </div>

      <div class="info">
        <div class="label">Total value:</div>
        <div class="value">{{ king.totalValue }}</div>
      </div>
    </div>

    <button class="back-button" @click="goBack">Back</button>
  </div>
</template>
<script>
import KingAPhoto from '@/static/king-1.png'
import KingBPhoto from '@/static/king-2.png'
import KingCPhoto from '@/static/king-3.png'

export default {
  mounted() {
  const donatedCoins = JSON.parse(this.$route.query.donatedCoins);
  this.kings = Object.entries(donatedCoins).map(([name, coins]) => ({
    name,
    coins,
    photo: this.getKingPhoto(name),
    totalValue: coins.reduce((total, coin) => total + coin.coin.value, 0)
  }));
  this.kings.sort((a, b) => b.totalValue - a.totalValue);
},

  data() {
    return {
      kings: []
    };
  },
  methods: {
    goBack() {
      this.$router.push({ name: 'index' });
    },
    getKingPhoto(name) {
      switch (name) {
        case 'King A':
          return KingAPhoto
        case 'King B':
          return KingBPhoto
        case 'King C':
          return KingCPhoto
        default:
          return null
      }
    }
  }
};
</script>

<style scoped>
.summary {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f7f7f7;
  border-radius: 5px;
}

h1 {
  font-size: 2rem;
  margin-bottom: 20px;
}

h2 {
  font-size: 1.5rem;
  margin-bottom: 10px;
}
.info {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin-bottom: 10px;
}

.label {
  font-size: 1.2rem;
  font-weight: bold;
  margin-right: 10px;
  text-transform: uppercase;
}
.value {
  font-size: 1.2rem;
  margin-right: 10px;
}

img {
  max-width: 100px;
  max-height: 100px;
  object-fit: cover;
}
.back-button {
  background-color: #444;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
}

.back-button:hover {
  background-color: #333;
}
</style>
