<template>
  <nav>
    <div class="nav-container">
      <div class="logo">
        <a href="/">
          <img src="/algonquin-pet-store.png" alt="Store Logo" class="logo-img">
          <span class="brand-name">Cloud Native Store</span>
        </a>
      </div>

      <button class="hamburger" @click="toggleNav" aria-label="Menu">
        <span class="hamburger-box">
          <span class="hamburger-inner" :class="{ 'open': isNavOpen }"></span>
        </span>
      </button>

      <ul class="nav-links" :class="{ 'nav-links--open': isNavOpen }">
        <li>
          <router-link to="/" @click="closeNav" class="nav-item">Products</router-link>
        </li>
        <li>
          <router-link to="/cart" @click="closeNav" class="nav-item cart-btn">
            Cart
            <span v-if="cartItemCount > 0" class="cart-badge">{{ cartItemCount }}</span>
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
/* --- NAVIGATION BAR --- */
nav {
  background-color: #0046be; /* Best Buy Blue */
  color: #fff;
  height: 80px;
  width: 100%;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  display: flex;
  align-items: center;
}

.nav-container {
  max-width: 1200px;
  width: 100%;
  margin: 0 auto;
  padding: 0 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* --- LOGO AREA --- */
.logo a {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: white;
  gap: 15px;
}

.logo-img {
  height: 45px;
  width: auto;
  transition: transform 0.2s ease;
}

.logo:hover .logo-img {
  transform: scale(1.05);
}

.brand-name {
  font-family: 'Inter', sans-serif;
  font-weight: 800;
  font-size: 1.2rem;
  letter-spacing: -0.5px;
  text-transform: uppercase;
}

/* --- LINKS --- */
.nav-links {
  display: flex;
  list-style: none;
  gap: 2rem;
  margin: 0;
  padding: 0;
  align-items: center;
}

.nav-item {
  text-decoration: none;
  color: rgba(255, 255, 255, 0.9);
  font-weight: 600;
  font-size: 1rem;
  transition: all 0.2s ease;
  padding: 8px 0;
  position: relative;
}

.nav-item:hover, .router-link-active {
  color: #ffffff;
}

.nav-item::after {
  content: '';
  position: absolute;
  width: 0;
  height: 2px;
  bottom: 0;
  left: 0;
  background-color: #ffce00; /* Yellow accent */
  transition: width 0.3s;
}

.nav-item:hover::after {
  width: 100%;
}

/* --- CART BUTTON --- */
.cart-btn {
  display: flex;
  align-items: center;
  gap: 8px;
}

.cart-badge {
  background-color: #ffce00; /* Best Buy Yellow */
  color: #0046be; /* Blue Text */
  font-size: 0.8rem;
  font-weight: 800;
  padding: 2px 8px;
  border-radius: 12px;
  min-width: 20px;
  text-align: center;
}

/* --- HAMBURGER MENU (Mobile) --- */
.hamburger {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 10px;
}

.hamburger-box {
  width: 30px;
  height: 24px;
  display: inline-block;
  position: relative;
}

.hamburger-inner {
  display: block;
  top: 50%;
  margin-top: -2px;
  width: 30px;
  height: 3px;
  background-color: #fff;
  position: absolute;
  transition-property: transform;
  transition-duration: 0.15s;
  transition-timing-function: ease;
  border-radius: 4px;
}

.hamburger-inner::before, .hamburger-inner::after {
  width: 30px;
  height: 3px;
  background-color: #fff;
  border-radius: 4px;
  position: absolute;
  transition-property: transform;
  transition-duration: 0.15s;
  transition-timing-function: ease;
  content: "";
  display: block;
}

.hamburger-inner::before {
  top: -10px;
}

.hamburger-inner::after {
  bottom: -10px;
}

/* --- RESPONSIVE DESIGN --- */
@media (max-width: 768px) {
  .hamburger {
    display: block;
  }

  .nav-links {
    position: absolute;
    top: 80px;
    left: 0;
    right: 0;
    background-color: #003da6; /* Slightly darker blue for menu */
    flex-direction: column;
    align-items: center;
    gap: 0;
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.3s ease-out;
    box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  }

  .nav-links--open {
    max-height: 300px;
    padding-bottom: 1rem;
  }

  .nav-links li {
    width: 100%;
    text-align: center;
  }

  .nav-item {
    display: block;
    padding: 1.5rem;
    border-bottom: 1px solid rgba(255,255,255,0.1);
  }
  
  .nav-item::after {
    display: none;
  }
}
</style>
