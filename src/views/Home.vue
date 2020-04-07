<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2> {{name}}</h2>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>Price: <input type="text" v-model="newProductPrice"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <p>Image URL: <input type="text" v-model="newProductImageUrl"></p>
    <button v-on:click="addProduct()">Create</button>
    <div v-bind:key="product.id" v-for="product in products">
      <p>{{ product.name }}</p>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      name: "Ryan",
      products:[],
      newProductName: "",
      newProductPrice: null,
      newProductDescription: "",
      newProductImageUrl: ""
    };
  },
  created: function() {
    console.log("I was auto-created");
    axios.get("/api/products").then(response => {
      console.log("I'm inside the callback");
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {
    addProduct: function() {

      console.log("This should add a product");
      
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        image_url: this.newProductImageUrl
      };

      axios.post("/api/products", params).then(response => {
        console.log(response.data);
        this.products.push(response.data);
        this.newProductName = "";
        this.newProductPrice = "";
        this.newProductDescription = "";
        this.newProductImageUrl = "";
      });
    }
  }
};
</script>