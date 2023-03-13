
<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <a class="navbar-brand" href="#">Logo</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav"
        aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link" to="/register">Register</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/login">Login</router-link>
          </li>
        </ul>
      </div>
    </nav>

    <div class="container">
      <input type="text" v-model="searchTerm" placeholder="Buscar producto...">
      <h1 class="text-center my-5">Lista de productos</h1>

      <div class="row row-cols-1 row-cols-md-3 g-4">
        <div v-for="product in filteredProducts" :key="product.id" class="col">
          <div class="card h-100">
            <div class="card-header"><img :src="product.image" class="card-img-top" alt="..." height="300" width="150"></div>
            <div class="card-body">
              <h5 class="card-title">{{ product.title }}</h5>
            <p class="card-text">{{ product.category }}</p>
          </div>
          <div class="card-footer">
            <small class="text-muted">{{ product.price }}</small>
          </div>
        </div>
      </div>
      <!-- <div class="col">
          <div class="card h-100">
            <img src="https://via.placeholder.com/300x200" class="card-img-top" alt="...">
            <div class="card-body">
              <h5 class="card-title">Producto 2</h5>
              <p class="card-text">Descripción del producto 2.</p>
            </div>
            <div class="card-footer">
              <small class="text-muted">$59.</small>
            </div>
          </div>

        </div>
        <div class="col">
          <div class="card h-100">
            <img src="https://via.placeholder.com/300x200" class="card-img-top" alt="...">
            <div class="card-body">
              <h5 class="card-title">Producto 2</h5>
                <p class="card-text">Descripción del producto 2.</p>
              </div>
              <div class="card-footer">
                <small class="text-muted">$59.</small>
              </div>
            </div>

          </div> -->
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';

// eslint-disable-next-line @typescript-eslint/no-unused-vars
interface Product {
  id: number;
  title: string;
  price: number;
  description: string;
  category: string;
  image: string;
  rating: {
    rate: number;
    count: number;
  }
}

export default defineComponent({
  name: 'SignUpForm',
  data() {
    return {
      products: [] as Product[],
      searchTerm: '' 
    };


  },
  created() {
    this.listProducts()
  },
 
  methods: {
    // eslint-disable-next-line @typescript-eslint/no-empty-function
    async listProducts() {
      await axios.get('https://fakestoreapi.com/products')
        .then(response => {
          this.products = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  computed: {
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    filteredProducts():any {
      return this.products.filter((product:Product) => {
        return product.category.toLowerCase().includes(this.searchTerm.toLowerCase())
      })
    }
  }    
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
