<template>
  <div class="page">
    <div
      v-for="(king, index) in kings"
      :key="index"
      class="page-king"
    >
      <h2
        @click="selectKing(king)"
        :class="{ 'selected': king === selectedKing }"
        class="page-king-name"
      >
        <img
          :src="king.photo"
          class="page-selected-photo"
          v-if="king === selectedKing"
        />
        <img :src="king.photo" class="page-king-photo" v-else />
        <p>{{ king.name }}</p>
      </h2>
      <div class="page-coins-container">
        <div
          v-for="coin in availableCoins"
          :key="coin.id"
          @click="donateCoin(king, coin)"
          :class="{ 'silver': coin.value === 1, 'golden': coin.value === 2 }"
          class="page-coin"
        >
          {{ coin.value }} coin
        </div>
      </div>
      <div class="page-coins-container">
        <div
          v-for="(coin, coinIndex) in donatedCoins[king.name]"
          :key="coinIndex"
          @click="revokeDonation(king, coin)"
          :class="{ 'silver_rev': coin.coin.value === 1, 'golden_rev': coin.coin.value === 2 }"
          class="page-donated-coin"
        >
          {{ coin.coin.value }} coin
        </div>
      </div>
    </div>
    <button
      @click="submitDonation"
      :disabled="!canSubmit"
      class="page-submit-button"
    >
      Submit
    </button>
  </div>
</template>

<script>
import KingAPhoto from '@/static/king-1.png'
import KingBPhoto from '@/static/king-2.png'
import KingCPhoto from '@/static/king-3.png'
export default {
  props: {
    donationCoins: {
      type: Object,
      default: () => ({})
    }
  },
  data() {
    return {
      kings: [
        { name: 'King A', photo: KingAPhoto },
        { name: 'King B', photo: KingBPhoto },
        { name: 'King C', photo: KingCPhoto },
      ],

      selectedKing: null,
      availableCoins: [
        { id: 1,value: Math.floor(Math.random() * 2) + 1  },
        { id: 2,value: Math.floor(Math.random() * 2) + 1  },
        { id: 3,value: Math.floor(Math.random() * 2) + 1  },
        { id: 4,value: Math.floor(Math.random() * 2) + 1  },
        { id: 5,value: Math.floor(Math.random() * 2) + 1  },
        { id: 6,value: Math.floor(Math.random() * 2) + 1  },
        { id: 7,value: Math.floor(Math.random() * 2) + 1  },
        { id: 8,value: Math.floor(Math.random() * 2) + 1  },
        { id: 9,value: Math.floor(Math.random() * 2) + 1  },
      ],
      donatedCoins: {
        'King A': [],
        'King B': [],
        'King C': [],
      },
    }
  },
  computed: {
    totalDonatedCoins() {
      return this.donatedCoins[this.kings[0].name].length +
        this.donatedCoins[this.kings[1].name].length +
        this.donatedCoins[this.kings[2].name].length
    },
    canSubmit() {
      return this.availableCoins.length === 0 && this.selectedKing !== null
    },
  },
  methods: {
    selectKing(king) {
      this.selectedKing = king
    },
    donateCoin(king, coin) {
      if (this.selectedKing !== null && !this.donatedCoins[king.name].includes(coin)) {
        this.availableCoins = this.availableCoins.filter(c => c.id !== coin.id)
        const donatedCoin = { king: king.name, coin }
        this.donationCoins[coin.id] = donatedCoin
        this.donatedCoins[king.name].push(donatedCoin)

      }
    },
    revokeDonation(king, coin) {
      this.donatedCoins[king.name] = this.donatedCoins[king.name].filter(c => c !== coin)
      this.availableCoins.push(coin.coin)
    },
    submitDonation() {
  this.$router.push({ name: 'submit', query: { donatedCoins: JSON.stringify(this.donatedCoins) } })
}

  },
}
</script>

<style>
.page {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, sans-serif;
}
.page-king {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 1rem;
}

.page-king-name {
  font-size: 1.5rem;
  cursor: pointer;
  margin-bottom: 1rem;
  color: #333;
}

.page-king-name.selected {
  color: #3300ff;
}

.page-coins-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 1rem;
}
.page-king-photo {
  width: 100px;
  height: 100px;
}
.page-selected-photo {
  width: 150px;
  height: 150px;
}

.page-coin,
.page-donated-coin {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 3rem;
  height: 3rem;
  border-radius: 50%;
  margin-right: 1rem;
  font-size: 1rem;
  color: white;
  cursor: pointer;
}

.page-coin.silver {
  background-color: #aaa9ad;
}

.page-coin.golden {
  background-color: #d4af37;
}
.page-donated-coin.silver_rev {
  background-color: #6f6d78;
}

.page-donated-coin.golden_rev {
  background-color: #ebdaa2;
}

.page-donated-coin {
  background-color: #007bff;
}

.page-submit-button {
  padding: 1rem;
  font-size: 1rem;
  border-radius: 0.5rem;
  background-color: #042907;
  color: white;
  border: none;
  cursor: pointer;
}

.page-submit-button:disabled {
  background-color: #b38a8a;
  cursor: default;
}
</style>
