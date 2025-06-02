
     <template>
  <div>
    <!-- EN-TÊTE -->
    <header class="header">
      <div class="logo-section">
        <img src="https://via.placeholder.com/50" alt="Logo" class="logo" />
        <h1>Cosmétique Pro</h1>
      </div>
      <nav class="nav-bar">
        <a href="#">Accueil</a>
        <a href="#">Produits</a>
        <a href="#">Contact</a>
      </nav>
    </header>

    <!-- CONTENU PRINCIPAL -->
    <div class="container">
      <input
        type="text"
        v-model="search"
        placeholder="Rechercher un produit..."
        class="search-input"
      />

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
  </div>
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

       .header {
          background-color: #007BFF;
          color: white;
          padding: 15px 20px;
          display: flex;
          justify-content: space-between;
          align-items: center;
          flex-wrap: wrap;
        }

        .logo-section {
          display: flex;
          align-items: center;
          gap: 10px;
        }

        .logo {
          border-radius: 50%;
          width: 50px;
          height: 50px;
        }

        .nav-bar a {
          color: white;
          margin-left: 20px;
          text-decoration: none;
          font-weight: bold;
        }

        .nav-bar a:hover {
          text-decoration: underline;
        }

</style>





