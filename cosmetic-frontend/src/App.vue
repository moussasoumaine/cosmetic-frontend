<template>

      <div class="container">
        <h1>Catalogue de Produits Cosmétiques 💄</h1>

        <!-- Barre de recherche -->
        <input
          type="text"
          v-model="search"
          placeholder="Rechercher un produit..."
          class="search-input"
        />

        <!-- Liste des produits filtrée -->
        <div class="products">
          <div
            class="card"
            v-for="product in filteredProducts"
            :key="product.id"
          >
            <img :src="product.image" alt="Image du produit" />
            <h2>{{ product.name }}</h2>
            <p>{{ product.description }}</p>
            <p><strong>{{ product.price }} €</strong></p>
            <p>Catégorie : {{ product.category }}</p>
            <p>Marque : {{ product.brand }}</p>
          </div>
        </div>
      </div>

      <!-- 
      <div class="container">
        <h1>Bievenue Dans notre site officiel💄</h1>
        <h1>Catalogue de Produits Cosmétiques 💄</h1>
        <div class="products">
          <div class="card" v-for="product in products" :key="product.id">
            <img :src="product.image" alt="Image du produit" />
            <h2>{{ product.name }}</h2>
            <p>{{ product.description }}</p>
            <p><strong>{{ product.price }} €</strong></p>
            <p>Catégorie : {{ product.category }}</p>
            <p>Marque : {{ product.brand }}</p>
          </div>
        </div>
      </div>
      -->
      

</template>



<script>
    import { products } from './products.js'

    export default {
      name: 'App',
      data() {
        return {
          products,
          search: ""
        }
      },
      computed: {
        filteredProducts() {
          const query = this.search.toLowerCase()
          return this.products.filter(product =>
            product.name.toLowerCase().includes(query)
          )
        }
      }
    }
</script>





<style>
      .container {
        max-width: 1000px;
        margin: auto;
        padding: 20px;
        font-family: Arial, sans-serif;
      }

      .products {
        display: flex;
        flex-wrap: wrap;
        gap: 20px;
      }

      .card {
        width: 250px;
        border: 1px solid #ccc;
        border-radius: 8px;
        padding: 15px;
        box-shadow: 2px 2px 10px rgba(0,0,0,0.1);
      }

      .card img {
        width: 100%;
        height: auto;
        border-radius: 4px;
      }

      .search-input {
        width: 100%;
        padding: 10px;
        margin-bottom: 20px;
        font-size: 16px;
        border-radius: 8px;
        border: 1px solid #aaa;
       }
</style>





