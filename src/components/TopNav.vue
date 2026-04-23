<template>
  <nav class="top-nav">
    <div class="top-strip"></div>
    <div class="nav-main">
      <div class="brand-wrap">
        <router-link to="/" class="logo" @click="closeNav">BEST BUY</router-link>
      </div>
      <div class="nav-center">Simple electronics deals every day</div>
      <button class="hamburger" @click="toggleNav">
        <span class="hamburger-icon"></span>
      </button>
      <ul class="nav-links" :class="{ 'nav-links--open': isNavOpen }">
        <li><router-link to="/" @click="closeNav">Shop</router-link></li>
        <li>
          <router-link to="/cart" class="cart-link" @click="closeNav">
            Cart
            <span class="cart-count">{{ cartItemCount }}</span>
          </router-link>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'TopNav',
  props: ['cartItemCount'],
  data() {
    return {
      isNavOpen: false
    }
  },
  methods: {
    toggleNav() {
      this.isNavOpen = !this.isNavOpen
    },
    closeNav() {
      this.isNavOpen = false
    }
  }
}
</script>

<style scoped>
.top-nav {
  background: #0046be;
  color: #fff;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  box-shadow: 0 8px 20px rgba(0, 26, 87, 0.3);
}

.top-strip {
  height: 8px;
  background: #ffe000;
}

.nav-main {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0.7rem 1rem;
  display: grid;
  grid-template-columns: auto 1fr auto;
  align-items: center;
  gap: 1rem;
}

.brand-wrap {
  display: flex;
  align-items: center;
}

.logo {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 112px;
  min-height: 48px;
  background: #ffe000;
  color: #1a1a1a;
  font-family: 'Oswald', Impact, sans-serif;
  font-size: 1.45rem;
  font-weight: 600;
  letter-spacing: 0.05em;
  text-decoration: none;
  padding: 0.2rem 0.8rem;
  border-radius: 2px;
}

.logo:hover {
  text-decoration: none;
}

.nav-center {
  font-size: 0.95rem;
  font-weight: 500;
  color: #e8efff;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.nav-links {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 1rem;
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-links a {
  color: #fff;
  text-decoration: none;
  font-weight: 600;
  padding: 0.35rem 0.45rem;
  border-radius: 6px;
}

.nav-links a:hover {
  background: rgba(255, 255, 255, 0.12);
  text-decoration: none;
}

.cart-link {
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
}

.cart-count {
  min-width: 24px;
  height: 24px;
  border-radius: 999px;
  background: #ffe000;
  color: #1f2937;
  font-size: 0.85rem;
  font-weight: 700;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.hamburger {
  display: none;
  grid-column: 3;
  background: none;
  border: 1px solid rgba(255, 255, 255, 0.38);
  border-radius: 6px;
  cursor: pointer;
  box-shadow: none;
  padding: 0.45rem;
  margin: 0;
}

.hamburger-icon {
  display: block;
  width: 24px;
  height: 3px;
  background-color: #fff;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
}

.hamburger-icon::before,
.hamburger-icon::after {
  content: '';
  display: block;
  width: 24px;
  height: 3px;
  background-color: #fff;
  position: absolute;
  left: 0;
}

.hamburger-icon::before {
  top: -6px;
}

.hamburger-icon::after {
  bottom: -6px;
}

@media (max-width: 768px) {
  .nav-main {
    grid-template-columns: auto 1fr auto;
  }

  .nav-center {
    display: none;
  }

  .nav-links {
    display: none;
    position: absolute;
    top: 56px;
    left: 0;
    right: 0;
    background: #00399c;
    padding: 1rem;
    border-top: 1px solid rgba(255, 255, 255, 0.2);
    box-shadow: 0 12px 20px rgba(0, 26, 87, 0.25);
  }

  .nav-links--open {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 1rem;
  }

  .logo {
    min-width: 96px;
    min-height: 42px;
    font-size: 1.25rem;
  }

  .hamburger {
    display: block;
  }
}
</style>