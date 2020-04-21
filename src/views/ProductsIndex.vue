<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <p> {{ product.name }} </p>
      <p> {{ product.price }} </p>
      <p> {{ product.description }} </p>
      <hr>
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
      message: "Welcome to the Mini Capstone Index Page!!",
      products: [],
    };
  },
  created: function() {
    console.log("This is in the created function");
    axios.get('/api/products').then(response => {
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {}
};
</script>