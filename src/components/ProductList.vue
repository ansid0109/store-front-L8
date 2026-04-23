<template>
  <section>
    <header class="hero-banner">
      <p class="hero-kicker">Featured deals</p>
      <h1>Tech for less. Delivered fast.</h1>
      <p class="hero-text">Browse this week's simple picks and add items to your cart in one click.</p>
    </header>

    <div class="product-list">
      <ProductCard
          v-for="product in products"
          :key="product.id"
          :product="product"
          @addToCart="addToCart"
        />
    </div>
  </section>
</template>

<script>
  import ProductCard from '../components/ProductCard'

  export default {
    name: 'ProductList',
    props: ['products'],
    components: {
      ProductCard
    },
    methods: {
      addToCart({ productId, quantity }) {
        this.$emit('addToCart', {
          productId: productId,
          quantity: quantity
        })
      }
    }
  }
</script>

<style scoped>
.hero-banner {
  background: linear-gradient(110deg, #003da8, #0052d4 65%, #0a65f0 100%);
  color: #fff;
  border-radius: 12px;
  padding: 1.35rem 1.3rem;
  margin: 0 0 1.1rem;
  border: 1px solid rgba(255, 255, 255, 0.24);
  box-shadow: 0 16px 28px rgba(0, 37, 120, 0.22);
}

.hero-kicker {
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-weight: 700;
  font-size: 0.74rem;
  color: #ffe775;
}

.hero-banner h1 {
  margin: 0.35rem 0;
  font-size: 1.75rem;
  line-height: 1.2;
}

.hero-text {
  margin: 0;
  color: #e8efff;
  font-size: 0.98rem;
}

.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1rem;
}

@media (max-width: 768px) {
  .hero-banner h1 {
    font-size: 1.35rem;
  }

  .hero-text {
    font-size: 0.9rem;
  }

  .product-list {
    grid-template-columns: 1fr;
  }
}
</style>