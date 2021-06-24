<template>
  <div class="home">
    <div v-if="products" class="product">
      <h1>Unsere Produkte</h1>
      <div v-for="product in products" :key="product.id" class="product">
        <div class="product-header">
          {{ product.name }} - {{ product.description }}
        </div>
        <div class="product-footer">{{ product.price }} €</div>
        <div class="actions">
          <!--   <button @click="editProduct(product.id)">Edit</button> -->
          <button @click="deleteProduct(product.id)">Delete</button>
        </div>
      </div>
    </div>

    <div class="product-form">
      {{ name }} - {{ description }} - {{ price }}
      <form @submit.prevent="createProduct">
        <div class="form-control">
          <label>Name</label>
          <input v-model="name" type="text" />
        </div>
        <div class="form-control">
          <label>Beschreibung</label>
          <input v-model="description" type="text" />
        </div>
        <div class="form-control">
          <label>Preis</label>
          <input v-model="price" type="number" />
        </div>
        <div class="form-control">
          <button type="submit">Hinzufügen</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// axios importieren
import axios from "axios";
const url = "http://localhost:3000/products/";

export default {
  name: "Home",
  components: {},
  views: {},
  data() {
    return {
      products: "",
      name: "",
      description: "",
      price: null,
    };
  },
  mounted() {
    // getProducts aufrufen
    this.getProducts();
  },

  methods: {
    getProducts() {
      axios
        .get(url)
        .then((response) => {
          console.log(response.data);
          this.products = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    createProduct() {
      axios
        .post(url, {
          name: this.name,
          description: this.description,
          price: this.price
        })
        .then((response) => {
          console.log(response);
          this.products.push(response);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    deleteProduct(id) {
      axios
        .delete(url, id, {
          id: this.id,
          name: this.name,
          description: this.description,
          price: this.price,
        })
        .then((response) => {
          this.products.delete(id);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    /* editProduct(id) {
      axios
        .put(url, id, {
          id: this.id,
          name: this.name,
          description: this.description,
          price: this.price,
        })
        .then((response) => {
          this.products.put(id);
        });
    }, */
  },
};
</script>

<style>
.product {
  margin-bottom: 2rem;
}

.form-control {
  margin-bottom: 1rem;
}
</style>
