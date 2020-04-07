<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2> {{name}}</h2>
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
      products:[]
    };
  },
  created: function() {
    console.log("I was auto-created");
    axios.get("/api/products").then(response => {
      console.log("I'm inside the callback");
      console.log(response.data);
      this.products = response.data;
    })
  },
  methods: {}
};
</script>