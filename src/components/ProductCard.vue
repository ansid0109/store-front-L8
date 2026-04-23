<template>
  <div class="product-card">
    <router-link :to="`/product/${product.id}`" class="product-image-link">
      <img
        :src="getImageSrc(product.image)"
        :alt="product.name"
        class="product-image"
        @error="onImageError"
      >
    </router-link>
    <div class="product-content">
      <router-link :to="`/product/${product.id}`" class="title-link">
        <h2>{{ product.name }}</h2>
      </router-link>
      <p class="description">{{ product.description }}</p>
      <div class="product-details">
        <div class="product-price">
          <p class="price">${{ Number(product.price).toFixed(2) }}</p>
        </div>
        <div class="product-controls">
          <input type="number" v-model="quantity" min="1" class="quantity-input" />
          <button class="add-button" @click="addToCart">Add to Cart</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductCard',
  props: ['product'],
  data() {
    return {
      quantity: 1
    }
  },
  methods: {
    incrementQuantity() {
      this.quantity++
    },
    decrementQuantity() {
      if (this.quantity > 1) {
        this.quantity--
      }
    },
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
.product-card {
  background: #fff;
  border: 1px solid #dfe5f2;
  border-radius: 10px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  box-shadow: 0 8px 18px rgba(8, 34, 89, 0.08);
  transition: transform 0.18s ease, box-shadow 0.18s ease;
}

.product-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 14px 26px rgba(8, 34, 89, 0.14);
}

.product-image-link {
  display: block;
  background: #f8faff;
  border-bottom: 1px solid #eef2fb;
}

.product-image {
  width: 100%;
  height: 210px;
  object-fit: cover;
  display: block;
}

.product-content {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  padding: 0.95rem;
  height: 100%;
}

.title-link {
  text-decoration: none;
}

.title-link h2 {
  margin: 0;
  color: #0f2b67;
  font-size: 1.02rem;
  line-height: 1.3;
}

.description {
  margin: 0;
  color: #4e5a72;
  font-size: 0.9rem;
  line-height: 1.45;
  min-height: 58px;
}

.product-details {
  margin-top: auto;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  gap: 0.6rem;
}

.product-price .price {
  margin: 0;
  font-size: 1.32rem;
}

.product-controls {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.add-button {
  height: 38px;
  padding: 0 0.95rem;
  min-width: 120px;
}

@media (max-width: 768px) {
  .product-image {
    height: 190px;
  }
}
</style>