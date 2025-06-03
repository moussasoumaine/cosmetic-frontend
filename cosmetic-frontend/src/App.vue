
     <template>
  <div>
    <!-- EN-TÊTE -->
    <header class="header">
      <div class="logo-section">
        <img src="Logo.png" alt="Logo" class="logo" />
        <h1>Cosmétique Pro</h1>
      </div>
      <nav class="nav-bar">
        <a href="#"> 🏠Accueil</a>
        <a href="#"> 🛍️Produits</a>
        <a href="#"> ✉️Contact</a>
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
      <div class="intro">
        <h2>Bienvenue chez <span class="brand-name">Cosmétique Pro</span> 🌟</h2>
           <p>
            Chez <strong>Cosmétique Pro</strong>, nous croyons que prendre soin de soi est bien plus qu’un simple geste esthétique : 
            c’est un art de vivre. Nos produits de soin et nos parfums élégants sont pensés pour révéler votre beauté naturelle, tout en respectant votre santé et votre bien-être.
          </p>
          <p>
            Laissez-vous séduire par des soins doux, des formules sûres, et des senteurs inoubliables — parce que vous méritez l’excellence, tous les jours.
          </p>

        <!-- Bienfaits des cosmétiques -->
        <div class="benefits">
          <h2>✨ Bienfaits des cosmétiques</h2>
          <ul>
            <li>Hydrate et nourrit la peau en profondeur;</li>
            <li>Protège contre les agressions extérieures (soleil, pollution...);</li>
            <li>Améliore l’éclat du teint et réduit les imperfections;</li>
            <li>Parfume le corps avec des senteurs élégantes et durables;</li>
            <li>Favorise la confiance en soi et le bien-être au quotidien.</li>
          </ul>
        </div>
      </div>

      <!-- Filtres -->
        <div class="filters">
          <select v-model="selectedCategory">
            <option value="">Toutes les catégories</option>
            <option v-for="cat in categories" :key="cat" :value="cat">{{ cat }}</option>
          </select>

          <select v-model="selectedBrand">
            <option value="">Toutes les marques</option>
            <option v-for="brand in brands" :key="brand" :value="brand">{{ brand }}</option>
          </select>

          <label>Prix min:
            <input type="number" v-model.number="minPrice" />
          </label>
          <label>Prix max:
            <input type="number" v-model.number="maxPrice" />
          </label>
        </div>

          <!-- Produits -->
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
            search: "",
            selectedCategory: "",
            selectedBrand: "",
            minPrice: 0,
            maxPrice: 50000,
          }
        },
        computed: {
          categories() {
            // retourne les catégories uniques
            return [...new Set(this.products.map(p => p.category))]
          },
          brands() {
            // retourne les marques uniques
            return [...new Set(this.products.map(p => p.brand))]
          },
          filteredProducts() {
            return this.products.filter(product => {
              const matchSearch = product.name.toLowerCase().includes(this.search.toLowerCase())
              const matchCategory = this.selectedCategory === "" || product.category === this.selectedCategory
              const matchBrand = this.selectedBrand === "" || product.brand === this.selectedBrand
              const matchPrice = product.price >= this.minPrice && product.price <= this.maxPrice

              return matchSearch && matchCategory && matchBrand && matchPrice
            })
          }
        }
      }
</script>





<style>
      /* description */
      .intro {
        background-color: #f0f8ff;
        padding: 25px;
        margin: 20px 0;
        border-radius: 10px;
        font-family: 'Segoe UI', sans-serif;
      }

      .brand-name {
        color: #007BFF;
        font-weight: bold;
      }

        /* Produits */
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
        border: 1px solid #6f0e0e;
        border-radius: 8px;
        padding: 15px;
        box-shadow: 2px 2px 10px rgba(0,0,0,0.1);
      }

      .card img {
        width: 100%;
        height: auto;
        border-radius: 4px;
      }

        
      /* Bare de recherche */
      .search-input {
        width: 100%;
        padding: 10px;
        margin-bottom: 20px;
        font-size: 16px;
        border-radius: 8px;
        border: 1px solid #22057a;
       }

          /* L'entete */
       .header {
          background-color: #310678;
          color: white;
          padding: 15px 20px;
          display: flex;
          justify-content: space-between;
          align-items: center;
          flex-wrap: wrap;
          border-radius: 5px;
        }

           /* Logo */
        .logo-section {
          display: flex;
          align-items: center;
          gap: 10px;
        }

        .logo {
          border-radius: 70%;
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

        /* Bienfaits */

          .benefits h2 {
            color: #d35400;
            margin-bottom: 10px;
          }

          /* filtres : Categorie, prix, marque */
          .filters {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin: 15px 0;
            align-items: center;
          }

          .filters select,
          .filters input {
            padding: 8px;
            font-size: 14px;
            border-radius: 5px;
            border: 1px solid #100972;
          }


          

</style>





