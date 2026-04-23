<template>
  <div class="product-detail" v-if="productExists">
    <div class="product-image">
      <img :src="getImageSrc(product.image)" :alt="product.name" @error="onImageError" />
    </div>
    <div class="product-info">
      <h2>{{ product.name }}</h2>
      <small class="product-meta">SKU: {{ product.id }}</small>
      <p>{{ product.description }}</p>
      <div class="product-controls">
        <p class="price-wrap"><span class="price">${{ Number(product.price).toFixed(2) }}</span></p>
        <input type="number" v-model="quantity" min="1" class="quantity-input" />
        <button class="detail-add-button" @click="addToCart">Add to Cart</button>
      </div>
    </div>
  </div>
  <div class="product-detail" v-else>
    <img src="../assets/404.jpg" alt="Product not found" />
    <h3>Opps! That product was not found...</h3>
  </div>
</template>

<script>
export default {
  name: 'ProductDetail',
  props: ['products'],
  data() {
    return {
      quantity: 1
    }
  },
  computed: {
    product() {
      return this.products.find(product => product.id == this.$route.params.id);
    },
    productExists() {
      return !!this.product;
    }
  },
  methods: {
    addToCart() {
      // Add the product and quantity to the cart
      this.$emit('addToCart', {
        productId: this.product.id,
        quantity: this.quantity
      })
    },
    getImageSrc(imagePath) {
      return imagePath || '/placeholder.png'
    },
    onImageError(event) {
      event.target.src = '/placeholder.png'
    }
  }
}
</script>

<style scoped>
.product-detail {
  background: #fff;
  border: 1px solid #dfe5f2;
  border-radius: 12px;
  display: flex;
  align-items: flex-start;
  gap: 1.2rem;
  margin: 0.25rem 0;
  padding: 1rem;
  box-shadow: 0 12px 24px rgba(8, 34, 89, 0.1);
}

.product-image {
  flex: 1;
}

.product-image img {
  width: 100%;
  max-height: 420px;
  height: auto;
  object-fit: cover;
  border-radius: 8px;
  border: 1px solid #edf1fa;
}

.product-info {
  flex: 1;
  text-align: left;
}

.product-info h2 {
  font-size: 1.6rem;
  margin: 0;
  color: #0f2b67;
}

.product-meta {
  display: inline-block;
  margin: 0.4rem 0 0.7rem;
  color: #68748f;
}

.product-info p {
  font-size: 1rem;
  margin: 0 0 1rem;
  color: #4e5a72;
  line-height: 1.55;
}

.product-controls {
  display: flex;
  align-items: center;
  gap: 0.7rem;
}

.price-wrap {
  margin: 0;
}

.price-wrap .price {
  font-size: 1.8rem;
}

.detail-add-button {
  min-height: 46px;
  min-width: 156px;
  padding: 0 1.2rem;
  font-size: 1rem;
}

@media (max-width: 768px) {
  .product-detail {
    flex-direction: column;
    padding: 0.85rem;
  }

  .product-info h2 {
    font-size: 1.35rem;
  }

  .product-controls {
    flex-wrap: wrap;
  }
}
</style>