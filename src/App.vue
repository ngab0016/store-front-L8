<template>
  <div class="app-container">
    <TopNav :cartItemCount="cartItemCount"/>
    <div class="main-content">
      <router-view
        :products="products"
        :cartItems="cartItems"
        @addToCart="addToCart"
        @removeFromCart="removeFromCart"
        @submitOrder="submitOrder"
      ></router-view>
    </div>
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
      const existingCartItem = this.cartItems.find(
        item => item.product.id == productId
      )
      if (existingCartItem) {
        existingCartItem.quantity += quantity
      } else {
        const product = this.products.find(product => product.id == productId)
        this.cartItems.push({ product, quantity })
      }
    },
    removeFromCart(index) {
      this.cartItems.splice(index, 1)
    },
    submitOrder() {
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
/* --- GLOBAL VARIABLES (Best Buy Theme) --- */
:root {
  --primary-blue: #0046be;    /* Best Buy Blue */
  --accent-yellow: #ffce00;   /* Best Buy Yellow */
  --bg-color: #f0f2f5;        /* Light Gray Background */
  --card-bg: #ffffff;
  --text-dark: #1d252c;
  --text-light: #555;
  --font-main: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

/* --- RESET & BODY --- */
body {
  margin: 0;
  padding: 0;
  font-family: var(--font-main);
  background-color: var(--bg-color);
  color: var(--text-dark);
  -webkit-font-smoothing: antialiased;
}

#app {
  text-align: center;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.main-content {
  margin-top: 80px; /* Space for fixed header */
  padding: 2rem;
  max-width: 1200px;
  margin-left: auto;
  margin-right: auto;
  width: 100%;
  box-sizing: border-box;
}

/* --- BUTTONS --- */
button {
  background-color: var(--primary-blue);
  color: white;
  border: none;
  padding: 0.8rem 1.5rem;
  border-radius: 6px;
  font-weight: 600;
  font-size: 0.95rem;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

button:hover {
  background-color: #003da6;
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

button:active {
  transform: translateY(0);
}

/* --- PRODUCT GRID --- */
.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 2rem;
  padding: 1rem 0;
}

/* --- PRODUCT CARD --- */
.product-card {
  background-color: var(--card-bg);
  border-radius: 12px;
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  border: 1px solid rgba(0,0,0,0.05);
  box-shadow: 0 4px 6px rgba(0,0,0,0.02);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.product-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 12px 24px rgba(0,0,0,0.08);
  border-color: rgba(0, 70, 190, 0.2);
}

.product-card img {
  max-width: 80%;
  height: 180px;
  object-fit: contain;
  margin-bottom: 1.5rem;
}

.product-card h2 {
  font-size: 1.1rem;
  font-weight: 700;
  margin: 0 0 0.5rem 0;
  color: var(--text-dark);
  line-height: 1.4;
}

.product-card p {
  color: var(--text-light);
  font-size: 0.9rem;
  margin-bottom: 1rem;
}

.product-price {
  font-size: 1.4rem;
  font-weight: 800;
  color: var(--primary-blue);
  margin-bottom: 1rem;
  align-self: flex-start;
}

.product-controls {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: auto;
  gap: 10px;
}

.quantity-input {
  width: 50px;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  text-align: center;
  font-weight: 600;
}

/* --- SHOPPING CART --- */
.shopping-cart {
  background-color: white;
  padding: 2rem;
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0,0,0,0.05);
  max-width: 900px;
  margin: 0 auto;
}

.shopping-cart h2 {
  font-size: 1.8rem;
  color: var(--text-dark);
  margin-bottom: 1.5rem;
  border-bottom: 2px solid var(--accent-yellow);
  display: inline-block;
  padding-bottom: 0.5rem;
}

.shopping-cart-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0 10px;
}

.shopping-cart-table th {
  text-align: left;
  color: var(--text-light);
  font-weight: 600;
  padding: 0 1rem;
}

.shopping-cart-table td {
  background: #f9f9f9;
  padding: 1rem;
}

.shopping-cart-table td:first-child {
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
}

.shopping-cart-table td:last-child {
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
}

.checkout-button {
  background-color: var(--accent-yellow);
  color: black;
  font-weight: 800;
  font-size: 1.1rem;
  margin-top: 2rem;
  padding: 1rem 3rem;
  width: 100%;
  max-width: 400px;
}

.checkout-button:hover {
  background-color: #e6b800;
}

/* --- NAVIGATION OVERRIDES (If TopNav has specific styles) --- */
nav {
  background-color: var(--primary-blue);
  color: white;
  padding: 1rem 2rem;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

nav a {
  color: white;
  font-weight: 600;
  text-transform: uppercase;
  font-size: 0.9rem;
  letter-spacing: 0.5px;
}

/* --- FOOTER --- */
footer {
  background-color: #1d252c;
  color: #8898aa;
  padding: 2rem;
  margin-top: auto;
  font-size: 0.9rem;
}
</style>
