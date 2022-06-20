<template>
  <div class="cart-list">
    <!--":key" should be added for "v-for"-->
    <div v-for="item in renderList" :key="item.id">
      {{ item.title }}: {{ item.amount }}  Итого: {{item.sum}}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cart: [],
  },
  computed: {
    // function turned to camel case
    renderList() {
      //sorted cart items to avoid duplicated ids
     return Array.from(new Set(this.cart.map(s => s.title ))).map(title => {
       const currentProduct = this.cart.filter(product => product.title === title)
       const currentProductAmount = currentProduct.map(product => product.amount).reduce((sum, a) => sum + a, 0)
      return {
        title: title,
        amount: currentProductAmount,
        sum: currentProduct[0].price * currentProductAmount
      }
    })
    },
  },
};
</script>

<style>
.cart-list {
  display: flex;
  flex-direction: column-reverse;
}
</style>
