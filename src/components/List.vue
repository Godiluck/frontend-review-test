<template>
  <div class="product-list">
    <div class="card" v-for="product in products" :style="{width: cardsWidth + '%'}">
      <p class="card-title">{{ product.title }}</p>
      <img class="card-image" :src="product.image" alt="">
      <p class="card-price">Цена: {{ product.price }} {{ currency }}</p>

      <div>
        <input type="number" ref="amount" :id="product.id">
        <span>кг</span>
      </div>

      <button @click="addToCart(product)"> В корзину</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currency: String,
  },
  data() {
    return {
      products: [],
      width: window.innerWidth
    };
  },
  computed: {
    cardsWidth() {
      let width = this.width;
      let count = 1;
      //should compare same data types (number to number)
      if (width > 840) {
        count = 3;
      } else if (width > 420 && width < 840) {
        count = 2;
      }

      return 100 / count;
    },
  },
  methods: {
    onResize() {
      this.width = window.innerWidth
      this.cardsWidth()
    },
    startPricesMonitoring() {
      //interval changed on 2s
      setInterval(this.getList, 2000);
    },
    async getList() {
      //inlined variable to shorten code
      this.products = await this.$store.dispatch('getProductsList');
    },
    addToCart(product) {
      // turned let to const in order to save data from rewriting
      const currentProductAmount = this.$refs.amount.find((input) => input.id === product.id).value;
      if (currentProductAmount !== '') {
        const data = {
          amount: Number(currentProductAmount),
          price: product.price,
          title: product.title,
          id: product.id,
        };
        document.getElementById(product.id).value = '';
        //make emit instead of parent
        this.$emit('addToCart', data)
      }

    },
  },
  created() {
    this.startPricesMonitoring();
    window.addEventListener('resize', this.onResize)
  },
};
</script>

<style>
.product-list {
  padding: 10px;
}

.card {
  display: inline-block;
  width: 100%;
  border: 1px solid #908888;
  border-radius: 5px;
  text-align: center;
  padding: 10px;
}

.card-image {
  width: 100%;
}

button {
  padding: 5px;
  margin: 5px;
}
</style>
