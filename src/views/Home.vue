<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2> {{name}}</h2>

    <!-- inputs for create -->
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>Price: <input type="text" v-model="newProductPrice"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <p>Image URL: <input type="text" v-model="newProductImageUrl"></p>
    <button v-on:click="addProduct()">Create</button>

    <!-- products.each do |product| -->
    <div v-bind:key="product.id" v-for="product in products">
      <p>Name: {{ product.name }}</p>
      <p>Price: {{ product.price }}</p>

      <!-- show more info/update form after clicking  button -->
      <button v-on:click="showInfo(product)">Show more info</button>
      <div v-if="currentProduct === product">
        <p>Description: {{ product.description }}</p>
        <img v-bind:src="product.image_url">
        
        <!-- product.name etc. refers to the v-for loop param  -->
        <p> Name: <input type="text" v-model="product.name"></p>
        <p> Price: <input type="text" v-model="product.price"></p>
        <p> Description: <input type="text" v-model="product.description"></p>
        <p> Image URL: <input type="text" v-model="product.image_url"></p>

        <!-- product refers to the v-for loop param -->
        <button v-on:click="updateProduct(product)">Update This Product</button>
        <div>
          <button v-on:click="deleteProduct(product)">Delete Product</button>
        </div>
      </div>
      <hr>
    </div> <!-- END products.each do |product| -->
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Mini Capstone!",
      name: "Ryan",
      products:[],
      newProductName: "",
      newProductPrice: null,
      newProductDescription: "",
      newProductImageUrl: "",
      currentProduct: {}
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
    showInfo(product) {
      console.log("SHOWING MORE INFO...");
      console.log(product);
      this.currentProduct = product;
    },

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
    },

    updateProduct: function(theProduct) {
      // console.log(theProduct) displays updated info before running the patch request because we modify the product via the user forms and then pass that data into theProduct param
      console.log("updating the product...");
      console.log(theProduct);

      var params = {
        name: theProduct.name,
        price: theProduct.price,
        description: theProduct.description,
        image_url: theProduct.image_url
      };

      axios.patch(`/api/products/${theProduct.id}`, params).then(response=> {
        console.log(response.data);
        theProduct = response.data;
      });
    },

    deleteProduct: function(theProduct) {
      console.log("Deleting the product...");
      console.log(theProduct);
      axios.delete('/api/products/' + theProduct.id).then(response => {
        console.log(response.data)
        // remove this from the page
        // find the index of the product to be deleted. this.products refers to the array up in data
        var index = this.products.indexOf(theProduct);
        // delete at the index, if you don't pass the argument length of 1, it will delete all elements in the array after the index as well
        this.products.splice(index, 1);
      });
    }
  }
};
</script>