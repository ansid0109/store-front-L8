<template>
  <div class="shopping-cart" v-if="hasCartItems">
    <header class="cart-header">
      <h2>Your Cart</h2>
      <p>{{ cartItems.length }} item(s)</p>
    </header>
    <table class="shopping-cart-table">
      <thead>
        <tr>
          <th>Item</th>
          <th>Quantity</th>
          <th>Price</th>
          <th>Total</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in cartItems" :key="item.product.id">
          <td>{{ item.product.name }}</td>
          <td>{{ item.quantity }}</td>
          <td>{{ item.product.price }}</td>
          <td>{{ getItemTotal(item) }}</td>
          <td><button @click="removeFromCart(item)">Remove</button></td>
        </tr>
      </tbody>
    </table>

    <div class="cart-footer">
      <div class="order-total">
        <span>Order total</span>
        <strong>${{ cartGrandTotal }}</strong>
      </div>
      <button class="checkout-button" @click="submitOrder">Checkout</button>
    </div>
  </div>
  <div class="shopping-cart" v-else>
    <h3>Your shopping cart is empty</h3>
  </div>
</template>

<script>
export default {
  name: 'ShoppingCart',
  props: ['cartItems'],
  computed: {
    hasCartItems() {
      return this.cartItems.length > 0
    },
    cartGrandTotal() {
      const total = this.cartItems.reduce((sum, item) => {
        return sum + item.quantity * item.product.price
      }, 0)
      return total.toFixed(2)
    }
  },
  methods: {
    getItemTotal(item) {
      const quantity = item.quantity
      const price = item.product.price
      const total = quantity * price
      return total.toFixed(2)
    },
    removeFromCart(item) {
      const index = this.cartItems.indexOf(item)
      if (index > -1) {
        this.$emit('removeFromCart', index)
      }
    },
    submitOrder() {
      this.$emit('submitOrder')
    }
  }
}
</script>

<style scoped>
.shopping-cart {
  background: #fff;
  border: 1px solid #dfe5f2;
  border-radius: 12px;
  padding: 1rem;
  box-shadow: 0 12px 24px rgba(8, 34, 89, 0.1);
}

.cart-header {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  margin-bottom: 0.8rem;
}

.cart-header h2 {
  margin: 0;
  font-size: 1.4rem;
  color: #0f2b67;
}

.cart-header p {
  margin: 0;
  color: #5f6a82;
}

.shopping-cart-table {
  width: 100%;
  border-collapse: collapse;
  background: #fff;
}

.shopping-cart-table th,
.shopping-cart-table td {
  border-bottom: 1px solid #e8edf8;
  text-align: left;
  padding: 0.7rem 0.55rem;
}

.shopping-cart-table th {
  background: #f4f7ff;
  color: #193773;
  font-size: 0.86rem;
  text-transform: uppercase;
  letter-spacing: 0.04em;
}

.shopping-cart-table tbody tr:hover {
  background: #f9fbff;
}

.cart-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 1rem;
}

.order-total {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  color: #5f6a82;
}

.order-total strong {
  color: #0f2b67;
  font-size: 1.4rem;
}

.checkout-button {
  min-height: 42px;
  min-width: 118px;
}

@media (max-width: 768px) {
  .shopping-cart {
    overflow-x: auto;
  }

  .shopping-cart-table {
    min-width: 600px;
  }

  .cart-footer {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.8rem;
  }
}
</style>
