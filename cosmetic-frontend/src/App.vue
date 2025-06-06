

    
    
     <template class="template">
      <div class="main-site-wrapper">
      <div>
        <!-- EN-TÊTE -->
        <header class="header">
          <div class="logo-section">
            <img src="@/Logo.jpg" alt="" class="logo" />
            <h1>Cosmétique Pro ✨</h1>
          </div>
          <nav class="nav-bar">
            <a href="App.vue"> Accueil</a>
            <a href=""> Produits</a>
            <a href="contact.js"> Contact</a>
          </nav>

      <!-- Partie droite : profil + panier -->
      <div class="header-actions">
        <div class="profile" @click="showLogin = !showLogin">
          👤
          <div v-if="showLogin" class="dropdown-form">
            <h4>Connexion</h4>
            <input v-model="user.email" type="text" placeholder="Email" />
            <input v-model="user.password" type="password" placeholder="Mot de passe" />
            <button @click="loginUser">Se connecter</button>
            <hr />
            <h4>Inscription</h4>
            <input type="text" placeholder="Nom complet" />
            <input type="email" placeholder="Email" />
            <input type="password" placeholder="Mot de passe" />
            <button>S'inscrire</button>
          </div>
        </div>

        <div class="cart">
          🛒
          <span class="badge">{{ cart.length }}</span>
        </div>
      </div>
    </header>

    <!-- CONTENU PRINCIPAL -->
    <div class="container">
      <input
        type="text"
        v-model="search"
        placeholder=" 🔍Rechercher un produit..."
        class="search-input"
      />
            <!-- Bare interactive -->
              <div class="quick-menu" v-if="!search">
          <div class="menu-item" @mouseover="hoverItem = 'marques'" @mouseleave="hoverItem = ''">
            MARQUES
            <div v-if="hoverItem === 'marques'" class="tooltip">Découvrez toutes nos marques partenaires.</div>
          </div>

          <div class="menu-item" @mouseover="hoverItem = 'offre'" @mouseleave="hoverItem = ''">
            -30% OFFRE
            <div v-if="hoverItem === 'offre'" class="tooltip">
              Obtenez -30% pour tout achat de plus de 50 000 Fcfa
            </div>
          </div>

              <!-- Desriptif de bienvenu -->
          <div class="menu-item" v-for="item in categoriesMenu" :key="item.id"
              @mouseover="hoverItem = item.id" @mouseleave="hoverItem = ''">
            {{ item.name }}
            <div v-if="hoverItem === item.id" class="tooltip">{{ item.tooltip }}</div>
          </div>
        </div>

      <div class="intro" v-if="!search">
        <div class="intro-text">
          <h2>Bienvenue chez <span class="brand">Cosmétique Pro ✨</span></h2>
          <p>
            Chez <strong>Cosmétique Pro</strong>, nous croyons que prendre soin de soi est bien plus qu’un simple geste esthétique :
            c’est un art de vivre. Nos produits de soin et nos parfums élégants sont pensés pour révéler votre beauté naturelle,
            tout en respectant votre santé et votre bien-être.
          </p>
          <p>
            Laissez-vous séduire par des soins doux, des formules sûres, et des senteurs inoubliables — parce que vous méritez l’excellence, tous les jours.
          </p>

          <div class="bienfaits">
            <h3>✨ Bienfaits des cosmétiques</h3>
            <ul>
              <li>Hydrate et nourrit la peau en profondeur</li>
              <li>Protège contre les agressions extérieures (soleil, pollution...)</li>
              <li>Améliore l’éclat du teint et réduit les imperfections</li>
              <li>Parfume le corps avec des senteurs élégantes et durables</li>
              <li>Favorise la confiance en soi et le bien-être au quotidien</li>
            </ul>
          </div>
          
        </div>

        <div class="intro-image">
          <img src="@/assets/image/aceuil3.jpg " alt="Beauté" />
         
          
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

        <!-- Tri par prix -->
          <div class="sort">
            <label for="sortOrder"> Trier par prix :</label>
            <select v-model="sortOrder" id="sortOrder">
              <option value="">-- Aucun tri --</option>
              <option value="asc">Prix croissant</option>
              <option value="desc">Prix décroissant</option>
            </select>
          </div>

          <!-- Produits -->
           <div class="products-wrapper">
              <button class="scroll-btn left" @click="scrollLeft">◀</button>

              <div class="products-grid" ref="productContainer">
                <ProductCard
                  v-for="product in filteredProducts"
                  :key="product.id"
                  :product="product"
                   @voir-fiche="showProduct"
                   @ajouter-au-panier="addToCart"
                   @payer="openPayment"
                />
              </div>

              <button class="scroll-btn right" @click="scrollRight">▶</button>
            </div>
    </div> 

          <!-- boutton de tous les produits -->
          <div class="see-all-wrapper">
            <button class="see-all-btn" @click="showOverlay = true">
              Voir tous les produits
            </button>
          </div>

  </div>   


          <!-- Overlay / Rideau -->
          <div v-if="showOverlay" class="overlay">
            <div class="overlay-content">
              <button class="close-btn" @click="showOverlay = false">✖</button>
              <h2>Liste complète des produits</h2>
              <input
                type="text"
                v-model="search"
                placeholder=" 🔍Rechercher un produit..."
                class="search-input"
              />
              <div class="overlay-grid">
                <ProductCard
                  v-for="product in filteredProducts"
                  :key="product.id + '-overlay'"
                  :product="product"
                   @voir-fiche="showProduct"
                   @ajouter-au-panier="addToCart"
                   @payer="openPayment"
                />
              </div>
              <!-- Bouton retour en haut dans le rideau -->
              <button class="scroll-top-btn-overlay" @click="scrollOverlayToTop">
                ⬆
              </button>

            </div>
          </div> <br>
          <!-- Fiche produit (popup) -->
            <div v-if="selectedProduct" class="product-popup">
              <div class="popup-content scrollable">
                <button class="close-btn" @click="selectedProduct = null"><strong>✖</strong></button>
                <img :src="selectedProduct.image" alt="Image produit" />
                <h2>{{ selectedProduct.name }}</h2>
                <p>{{ selectedProduct.description }}</p>
                <p><strong>{{ selectedProduct.price }} Fcfa</strong></p>
                <p>Catégorie : {{ selectedProduct.category }}</p>
                <p>Marque : {{ selectedProduct.brand }}</p>
                <button class="buy-btn" @click="openPayment(selectedProduct)">
                  <strong>🛒 Acheter maintenant</strong>
                </button>

                

              </div>
            </div>
             <!-- Boutton à remonté-->
            <button class="scroll-top-btn" v-show="showScrollTop" @click="scrollToTop">
              ⬆ 
            </button>



              <!-- footer Pied de page -->
        <footer class="footer">
          <div class="footer-container">

            <!-- Colonne 1 : Logo + description -->
            <div class="footer-column">
              <img src="@/Logo.jpg" alt="" class="footer-logo" />
              <h3>Cosmétique Pro ✨</h3>
              <p>
                " Cosmétique Pro est une plateforme qui vous propose des produits de soin et de parfumerie
                pour sublimer votre beauté et votre bien-être au quotidien ".
              </p>
            </div>

            <!-- Colonne 2 : Liens rapides -->
            <div class="footer-column">
              <h4>🔗 <U>Liens rapides</U></h4>
              <ul>
                <li><a href="#">🏠 Accueil</a></li>
                <li><a href="#">🛍️ Produits</a></li>
              </ul>
            </div>

            <!-- Colonne 3 : Ressources -->
            <div class="footer-column">
              <h4>📚 <U>Ressources</U></h4>
              <ul>
                <li><a href="#">🤝 Partenaires</a></li>
                <li><a href="#">💬 Témoignages</a></li>
              </ul>
            </div>

            <!-- Colonne 4 : Contact -->
            <div class="footer-column">
              <h4>📞 <U>Contactez-nous</U></h4>
              <ul>
                <li>📍 Adresse : Lomé, Togo</li>
                <li>📞 Tél : +228 90 00 00 00</li>
                <li>📧 Email : contact@cosmetiquepro.com</li>
              </ul>
            </div>

          </div>

            <!-- Bas de page -->
            <div class="footer-bottom">
              <p>&copy; 2025 Cosmétique Pro – Tous droits réservés. | <a href="#" class="poli"><strong>Politique de confidentialité</strong></a> | <a href="#" class="poli"><strong>Conditions d'utilisation</strong></a></p>
               <strong><p>Développé avec ❤️ par Moussa Soumaine Bachar</p></strong>
            </div>
          </footer>

        </div>

            <!-- fiche de paiement -->
        <div v-if="showPaymentForm" class="payment-overlay">
          <div class="payment-box">
            <button class="close-btn" @click="showPaymentForm = false">✖</button>
            <h2>Paiement du produit</h2>

            <p><strong>Produit :</strong> {{ selectedProduct?.name }}</p>
            <p><strong>Prix :</strong> {{ selectedProduct?.price }} Fcfa</p>

            <form @submit.prevent="confirmPayment">
              <input type="text" placeholder="Nom complet" required />
              <input type="email" placeholder="Adresse email" required />
              <input type="tel" placeholder="Téléphone" required />
              <select required>
                <option value="">Méthode de paiement</option>
                <option>Carte Bancaire</option>
                <option>Mobile Money</option>
                <option>PayPal</option>
              </select>

              <button type="submit" class="buy-btn">💳 <strong>Confirmer l’achat</strong></button>
            </form>
          </div>
        </div>

</template>




<script>
    import ProductCard from './components/ProductCard.vue'
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
            maxPrice: 500000,
            sortOrder: "" ,
            autoScroll: null,
            showOverlay: false,
            selectedProduct: null,
            showLogin: false,
            showScrollTop: false,
            showPaymentForm: false,
 // doit déjà existselectedProducter si on l'utilise

            cart: [],
          hoverItem: "",
          categoriesMenu: [
            { id: 'parfums', name: 'Parfums', tooltip: 'Explorez notre sélection de parfums élégants.' },
            { id: 'maquillage', name: 'Maquillage', tooltip: 'Maquillage pour toutes les carnations.' },
            { id: 'soin-visage', name: 'Soin visage', tooltip: 'Crèmes, sérums et soins du visage.' },
            { id: 'soin-corps', name: 'Soin corps', tooltip: 'Hydratation, gommage et plus.' },
            { id: 'cheveux', name: 'Cheveux', tooltip: 'Shampoings, soins capillaires.' },
            { id: 'nouveautes', name: 'NOUVEAUTÉS & TENDANCES', tooltip: 'Les derniers produits tendance !' },
            { id: 'cadeaux', name: 'Idées Cadeaux', tooltip: 'Offrez un soin ou un parfum.' },
            { id: 'conseils', name: 'Conseils', tooltip: 'Astuces beauté et soins quotidiens.' },
          ],

          user: {
            email: '',
            password: '',
            isLoggedIn: false
          }


          }

        },
        components: {
          ProductCard
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
            let filtered = this.products.filter(product => {
              const matchSearch = product.name.toLowerCase().includes(this.search.toLowerCase())
              const matchCategory = this.selectedCategory === "" || product.category === this.selectedCategory
              const matchBrand = this.selectedBrand === "" || product.brand === this.selectedBrand
              const matchPrice = product.price >= this.minPrice && product.price <= this.maxPrice

              return matchSearch && matchCategory && matchBrand && matchPrice
            })

            if (this.sortOrder === "asc") {
              filtered.sort((a, b) => a.price - b.price)
            } else if (this.sortOrder === "desc") {
              filtered.sort((a, b) => b.price - a.price)
            }

            return filtered
          }

        } ,
        methods: {
          scrollLeft() {
            this.$refs.productContainer.scrollBy({ left: -300, behavior: 'smooth' });
          },
          scrollRight() {
            const container = this.$refs.productContainer;

            // S'il est proche de la fin (10px ou moins)
            if (container.scrollLeft + container.clientWidth >= container.scrollWidth - 10) {
              // Revenir au début
              container.scrollTo({ left: 0, behavior: 'smooth' });
            } else {
              // Sinon, continuer vers la droite
              container.scrollBy({ left: 300, behavior: 'smooth' });
            }
          },
          showProduct(product) {
            this.selectedProduct = product;
          },
          addToCart(product) {
            this.cart.push(product);
          },
          loginUser() {
            if (this.user.email === 'test@mail.com' && this.user.password === '1234') {
              this.user.isLoggedIn = true;
              this.showLogin = false;
              alert('Connexion réussie !');
            } else {
              alert('Email ou mot de passe incorrect');
            }
          },
           // ... tes autres méthodes
          scrollToTop() {
            window.scrollTo({ top: 0, behavior: 'smooth' });
          },
          handleScroll() {
            this.showScrollTop = window.scrollY > 200;
          },
          scrollOverlayToTop() {
            const el = document.querySelector('.overlay');
            if (el) {
              el.scrollTo({ top: 0, behavior: 'smooth' });
            }
          },
          openPayment(product) {
            this.selectedProduct = product;
            this.showPaymentForm = true;
          },
          confirmPayment() {
            alert("✅ Paiement confirmé ! Merci pour votre achat.");
            this.showPaymentForm = false;
          }


        } ,
        // Mounted
        mounted() {
          this.autoScroll = setInterval(() => {
            this.scrollRight();
          }, 3000);
          window.addEventListener('scroll', this.handleScroll)
        }

      }
</script>





<style>
      /*dive de templete */
      .main-site-wrapper {
          position: relative;
          z-index: 1;
          overflow-x: hidden;
        }

        body, html {
          overflow-x: hidden;
          box-sizing: border-box;
          background-color:rgba(217, 211, 207, 0.1) ;
        }


      /* description */
        .intro {
          display: flex;
          align-items: flex-start;
          justify-content: space-between;
          gap: 40px;
          padding: 40px 20px;
          background-image: url('@/assets/image/aceuil.jpg');
          border-radius: 12px;
          margin-bottom: 30px;
        }

        .intro-text {
          flex: 2;
          font-size: 20px;
          color: #333;
          max-width: 60%;
        }

        .intro-text h2 {
          font-size: 30px;
          margin-bottom: 15px;

        }

        .intro-text .brand {
          color: #007BFF;
          font-size: 35px;
        }

        .bienfaits {
          margin-top: 30px;
          font-size: 20px;
        }

        .bienfaits h3 {
          color: #e67e22;
          margin-bottom: 10px;
          font-size: 30px;
        }

        .bienfaits ul {
          padding-left: 20px;
        }

        .intro-image {
          flex: 1;
          text-align: right;
        }

        .intro-image img {
          width: 150%;
          height: 550px;
          border-radius: 20px;
          box-shadow: 0 0 15px rgba(0,0,0,0.1);
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
        width: 200px;
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

        /* profile de connexion + Panier */
        .header-actions {
          display: flex;
          align-items: center;
          gap: 20px;
          position: relative;
        }

        .profile {
          cursor: pointer;
          position: relative;
          background-color: #f1f1f1;
          border-radius: 50%;
          width: 25px;
          height: 25px;
          
        }

        .dropdown-form {
          position: absolute;
          top: 40px;
          right: 0;
          background: white;
          color: black;
          padding: 15px;
          border-radius: 8px;
          width: 250px;
          z-index: 100;
          box-shadow: 0 0 10px rgba(0,0,0,0.2);
        }

        .dropdown-form input {
          width: 100%;
          margin-bottom: 10px;
          padding: 8px;
        }

        .dropdown-form button {
          width: 100%;
          background: #007BFF;
          color: white;
          border: none;
          padding: 8px;
          margin-bottom: 10px;
          border-radius: 5px;
          cursor: pointer;
        }

        .cart {
          position: relative;
          font-size: 20px;
          cursor: pointer;
        }

        .badge {
          background: red;
          color: white;
          font-size: 12px;
          padding: 2px 6px;
          border-radius: 50%;
          position: absolute;
          top: -8px;
          right: -10px;
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
            border-radius: 8px;
            border: 2px solid #b0420a;
          }

          .template {
            background-color: rgb(244, 456, 988);
          }

            /* tri par prix */
          .sort {
              margin: 15px 0;
              font-size: 14px;
            border-radius: 8px;
            border: 2px solid #b0420a;
            padding: 10px;
            }

            .sort label {
              margin-right: 10px;
              font-weight: bold;
              
            }

            /* footer Pied de page */
            .footer {
              background-color:  #310678;
              color: white;
              padding: 10px 5px 5px;
              font-family: 'Segoe UI', sans-serif;
              font-size: 12px;
              border-radius: 8px;
            }

            .footer-container {
              display: flex;
              flex-wrap: wrap;
              justify-content: space-between;
              gap: 30px;
              max-width: 1000px;
              margin: auto;
            }

            .footer-column {
              flex: 1 1 200px;
              min-width: 220px;
            }

            .footer-logo {
              width: 50px;
              height: 50px;
              border-radius: 50%;
              margin-bottom: 10px;
            }

            .footer-column h3,
            .footer-column h4 {
              margin: 10px 0;
            }

            .footer-column ul {
              list-style: none;
              padding: 0;
            }

            .footer-column li {
              margin: 6px 0;
            }

            .footer-column a {
              color: #fff;
              text-decoration: none;
            }

            .footer-column a:hover {
              text-decoration: underline;
            }

            .footer-bottom {
              text-align: center;
              margin-top: 5px;
              border-top: 1px solid yellow;
              padding-top: 10px;
              font-size: 11px;
            }
            .poli{
              color: yellow;
            }

              
            /* flesh de defiler */
            .products-wrapper {
              position: static;
              overflow: hidden;
              margin-top: 30px;
              z-index: 1;
              background-color: white;
            }

            .products-grid {
              display: grid;
              grid-auto-flow: column;
              grid-template-rows: repeat(2, auto); /* ✅ Deux lignes seulement */
              gap: 20px;
              overflow-x: auto;
              overflow-y: hidden;
              scroll-behavior: smooth;
              padding: 10px 40px;
              scrollbar-width: none;
              z-index: 1;
              position: static;
            }

            .products-grid::-webkit-scrollbar {
              display: none;
              z-index: 1;
              position: static;
            }

            .scroll-btn {
              position: absolute;
              top: 50%;
              transform: translateY(-50%);
              background-color: #007BFF;
              color: white;
              border: none;
              font-size: 20px;
              padding: 8px 12px;
              border-radius: 50%;
              cursor: pointer;
              z-index: 2;
            }

            .scroll-btn.left {
              left: 0;
            }

            .scroll-btn.right {
              right: 0;
            }

              /* Boutton d'affichage de toutes le sprosuits */
            .see-all-wrapper {
              text-align: center;
              margin-top: 20px;
            }

            .see-all-btn {
              background-color: #007BFF;
              color: white;
              border: none;
              padding: 10px 20px;
              font-size: 16px;
              border-radius: 8px;
              cursor: pointer;
            }

            .overlay {
              position: fixed;
              top: 0;
              left: 0;
              width: 100%;
              height: 100%;
              background-color: rgba(0,0,0,0.8);
              z-index: 1000;
              display: flex;
              justify-content: center;
              align-items: flex-start;
              overflow-y: auto;
              padding: 40px 20px;
            }

            .overlay-content {
              background: white;
              padding: 30px;
              border-radius: 10px;
              width: 90%;
              max-width: 1000px;
            }

            .overlay-grid {
              display: grid;
              grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
              gap: 30px; /* ✅ Ajoute plus d’espace entre les cartes */
              margin-top: 30px;
              padding: 10px;
            }


            .close-btn {
              position: sticky;
              top: 20px;
              right: 40px;
              background: #007BFF;
              color: white;
              border: none;
              font-size: 20px;
              border-radius: 50%;
              padding: 5px 10px;
              cursor: pointer;
            }

            /* Fiche de produits */
            .product-popup {
              position: fixed;
              top: 0;
              left: 0;
              width: 100%;
              height: 100%;
              background: rgba(0,0,0,0.7);
              display: flex;
              justify-content: center;
              align-items: center;
              z-index: 1001;
            }

            .popup-content {
              background: white;
              padding: 30px;
              border-radius: 10px;
              max-width: 400px;
              width: 90%;
              text-align: center;
              position: relative;
            }

            .popup-content img {
              width: 100%;
              border-radius: 8px;
              margin-bottom: 15px;
            }

            .popup-content h2 {
              margin-bottom: 10px;
            }
            .popup-content.scrollable {
              max-height: 90vh;
              overflow-y: auto;
            }
            .popup-content img {
              max-width: 100%;
              border-radius: 10px;
              margin-bottom: 15px;
            }

            .popup-content .buy-btn {
              margin-top: 30px;
              background-color: #28a745;
              border-radius: 5px;
              padding: 15px;
              color: #fdfdfd;
              font-size: 20px;
            }

            /* Bare interactive */
            .quick-menu {
              display: flex;
              flex-wrap: wrap;
              justify-content: center ;
              gap: 10px;
              margin: 5px 0;
              font-size: 10px;
            }

            .menu-item {
              position: relative;
              background-color: #f1f1f1;
              padding: 10px 15px;
              border-radius: 20px;
              cursor: pointer;
              font-weight: bold;
              transition: background 0.3s;
            }

            .menu-item:hover {
              background-color: #007BFF;
              color: white;
            }

            .tooltip {
              position: absolute;
              top: 45px;
              left: 50%;
              transform: translateX(-50%);
              background: #333;
              color: white;
              padding: 15px;
              border-radius: 10px;
              font-size: 20px;
              width: 200px;
              text-align:;
              z-index: 20;
            }

            /* Boutton a remonté */
            .scroll-top-btn {
              position: sticky;
              bottom: 20px;
              
              font-size: 20px;
              background: #e42c0f;
              color: white;
              border: none;
              padding: 20px 30px;
              border-radius: 50%;
              cursor: pointer;
              z-index: 999;
              box-shadow: 0 0 15px rgba(0,0,0,0.3);
              border-color: #333;
            }

            .product-popup,
            .overlay,
            .scroll-top-btn {
              z-index: 10;
            }

            iframe[src*="tidio"] {
              z-index: 99999 !important;
            }

            /* Bouton à remonté de rideau */
            .scroll-top-btn-overlay {
              position: sticky;
              bottom: 30px;
              right: 30px;
              font-size: 18px;
              background: #007BFF;
              color: white;
              border: none;
              padding: 15px 20px;
              border-radius: 50%;
              cursor: pointer;
              z-index: 1001;
              box-shadow: 0 0 10px rgba(0,0,0,0.3);
            }
            /* Fiche de paiement */
            .payment-overlay {
              position: fixed;
              top: 0;
              left: 0;
              width: 100%;
              height: 100%;
              background: rgba(0,0,0,0.7);
              z-index: 1002;
              display: flex;
              justify-content: center;
              align-items: center;
            }

            .payment-box {
              background-image: url('@/assets/image/aceuil2.webp');
              padding: 40px;
              border-radius: 10px;
              width: 100%;
              max-width: 400px;
              text-align: center;
              position: relative;
            }

            .payment-box form input,
            .payment-box form select {
              width: 100%;
              padding: 10px;
              margin: 10px 0;
            }

            .payment-box button {
              margin-top: 15px;
              padding: 10px;
              border-radius: 8px;
              background-color: ;
            }




</style>





