<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>New Product Form</h1>
    <div>
      Name: <input type="text" v-model="newProductName">
      Price: <input type="text" v-model="newProductPrice">
      Description: <input type="text" v-model="newProductDescription">
      Image: <input type="text" v-model="newProductImageUrl">
    <button v-on:click="createProduct()">Create Product</button>

    </div>
    <div v-for="product in products">
      <h2>{{ product.name }}</a></h2>
      <p>{{ product.description }}</p>
      <p>{{ product.price }}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name">
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
// let axios = require("axios");

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!", 
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: ""
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
},
  methods: {
    createProduct: function() {
      console.log("Product created!");

      let params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        image_url: this.newProductImageUrl
      }

      axios.post("/api/products", params).then(response => {
        this.products.push(response.data);
      }).catch(error => console.log(error.response));

      this.newProductName = "",
      this.newProductPrice = "",
      this.newProductDescription = "",
      this.newProductImageUrl = ""
    }
  }
};
</script>