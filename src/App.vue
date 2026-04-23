<template>
  <div class="app-shell">
    <TopNav :cartItemCount="cartItemCount" />
    <main class="app-content">
      <router-view
        :products="products"
        :cartItems="cartItems"
        @addToCart="addToCart"
        @removeFromCart="removeFromCart"
        @submitOrder="submitOrder"
      ></router-view>
    </main>
  </div>
</template>

<script>
import TopNav from './components/TopNav.vue'

export default {
  name: 'App',
  components: {
    TopNav
  },
  data() {
    return {
      cartItems: [],
      products: [],
    }
  },
  computed: {
    cartItemCount() {
      return this.cartItems.reduce((total, item) => {
        return total + item.quantity
      }, 0)
    }
  },
  mounted() {
    this.getProducts()
  },
  methods: {
    getProducts() {
      fetch('/products')
        .then(response => response.json())
        .then(products => {
          console.log('success getting proxy products')
          this.products = products
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while fetching products')
        })
    },
    addToCart({ productId, quantity }) {
      // check if the product is already in the cart
      const existingCartItem = this.cartItems.find(
        item => item.product.id == productId
      )
      if (existingCartItem) {
        // if it is, increment the quantity
        existingCartItem.quantity += quantity
      } else {
        // if not, find the product, and add it with quantity to the cart
        const product = this.products.find(product => product.id == productId)
        this.cartItems.push({ product, quantity })
      }
    },
    removeFromCart(index) {
      this.cartItems.splice(index, 1)
    },
    submitOrder() {
      // get the order-service URL from an environment variable
      // const orderServiceUrl = process.env.VUE_APP_ORDER_SERVICE_URL;

      // create an order object
      const order = {
        customerId: Math.floor(Math.random() * 10000000000).toString(),
        items: this.cartItems.map(item => {
          return {
            productId: item.product.id,
            quantity: item.quantity,
            price: item.product.price
          }
        })
      }

      console.log(JSON.stringify(order));

      // call the order-service using fetch
      fetch(`/order`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(order)
      })
        .then(response => {
          console.log(response)
          if (!response.ok) {
            alert('Error occurred while submitting order')
          } else {
            this.cartItems = []
            alert('Order submitted successfully')
          }
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while submitting order')
        })
    }
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&family=Oswald:wght@500;600&display=swap');

:root {
  --bb-blue: #0046be;
  --bb-deep-blue: #00308f;
  --bb-yellow: #ffe000;
  --ink: #1f2937;
  --muted: #566074;
  --surface: #f3f5f9;
  --card: #ffffff;
  --line: #d7ddec;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: linear-gradient(180deg, #f9fbff 0%, #f3f5f9 100%);
  color: var(--ink);
  font-family: 'Montserrat', 'Segoe UI', Tahoma, sans-serif;
}

#app {
  min-height: 100vh;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.app-shell {
  min-height: 100vh;
}

.app-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 120px 16px 32px;
}

a {
  color: var(--bb-blue);
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

button {
  border: 0;
  border-radius: 6px;
  background: var(--bb-yellow);
  color: #111;
  font-family: 'Montserrat', 'Segoe UI', Tahoma, sans-serif;
  font-weight: 700;
  cursor: pointer;
  transition: background-color 0.18s ease, transform 0.18s ease, box-shadow 0.18s ease;
}

button:hover {
  background: #ffe93d;
  transform: translateY(-1px);
  box-shadow: 0 8px 16px rgba(0, 41, 121, 0.14);
}

button:active {
  transform: translateY(0);
}

.quantity-input {
  width: 72px;
  height: 38px;
  border: 1px solid #bec7dd;
  border-radius: 6px;
  padding: 6px 8px;
  font-size: 0.95rem;
}

.price {
  color: #bb0628;
  font-weight: 700;
}

@media (max-width: 768px) {
  .app-content {
    padding-top: 96px;
  }
}
</style>
