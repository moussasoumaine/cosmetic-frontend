

<template>
    <div class="card">
        <img :src="product.image" alt="Image du produit" />
        <h2>{{ product.name }}</h2>
        <p>{{ product.description }}</p>
        <p><strong>{{ product.price }} Fçfa</strong></p>
        <p>Catégorie : {{ product.category }}</p>
        <p>Marque : {{ product.brand }}</p>

        <button class="buy-btn" @click="$emit('voir-fiche', product)">
        <strong>🛒 Acheter</strong>
        </button> <br>
        <button class="buy-btn" @click="$emit('ajouter-au-panier', product)">
        <strong>🛍️ Ajouter au panier</strong>
        </button>
        


   </div>
</template>

<script>
        export default {
        name: 'ProductCard',
        props: {
            product: Object
        },
        methods: {
            acheterProduit(nom) {
                alert(`Vous avez choisi d'acheter : ${nom}`);
            }
        }

        }
</script>


<style scoped>
        .card {
        width: 250px;
        height: auto;
        border: 1px solid #6f0e0e;
        border-radius: 8px;
        padding: 40px;
        box-shadow: 2px 2px 10px rgba(0,0,0,0.1);
        flex-shrink: 0;
        box-sizing: border-box;
       
      }

        .card img {
        width: 100%;
        height: auto;
        border-radius: 4px;
        
        }
        /* Boutton d'achat */
        .buy-btn {
            margin-top: 10px;
            background-color: #28a745;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 5px;
            font-size: 14px;
            cursor: pointer;
            width: 100%;
        }

         .buy-btn:hover {
            background-color: #218838;
         }

</style>
