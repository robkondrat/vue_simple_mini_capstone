<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>New Product Form</h1>
    <div class="new-form">
      Name: <input type="text" v-model="newProductName">
      Price: <input type="text" v-model="newProductPrice">
      Description: <input type="text" v-model="newProductDescription">
      Image: <input type="text" v-model="newProductImageUrl">
    <button v-on:click="createProduct()">Create Product</button>

    </div> <!-- end of .new-form -->

    <h1>All Products</h1>
    <h1>~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~</h1>

    <div v-for="product in products">
      <h2>{{ product.name }}</a></h2>
      <img v-on:click="showProduct(product)"v-bind:src="product.image_url" v-bind:alt="product.name">
      <div class="show-page" v-if="product === currentProduct">
        <h4>{{ product.price }}</h4>
        <p>{{ product.description }}</p>

        <div class="edit-form">
          <h4>Edit Product</h4>

          <div>
            Name: <input type="text" v-model="product.name">
          </div>

          <div>
            Price: <input type="text" v-model="product.price">
          </div>

          <div>
            Description: <input type="text" v-model="product.description">
          </div>

          <div>
            Image: <input type="text" v-model="product.image_url">
          </div>

          <button v-on:click="updateProduct(product)">Update</button>

        </div> <!-- end of .edit-form -->

        <div class="destroy-action">
          <button v-on:click="destroyProduct(product)">DESTROY!!</button>
        </div> 

      </div> <!-- end of .show-page -->
        <h1>~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~</h1>

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
      currentProduct: {},
      message: "What up!", 
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
    },

    showProduct: function(inputProduct) {
      if (this.currentProduct !== inputProduct) {
      this.currentProduct = inputProduct;} 
      else {
      this.currentProduct = {}}
    },

    updateProduct: function(inputProduct) {
      var clientParams = {
        name: inputProduct.name,
        price: inputProduct.price,
        description: inputProduct.description,
        image_url: inputProduct.image_url
      };

      axios
        .patch("/api/products/" + inputProduct.id, clientParams)
        .then(response => {
          console.log("Success", response.data);
        }).catch(error => {
          console.log(error.response.data);
        });
    },

    destroyProduct: function(inputProduct) {
      axios
        .delete("/api/products/" + inputProduct.id)
        .then(response => {
          console.log("Success", response.data);
          var index = this.products.indexOf(inputProduct);
          this.products.splice(index, 1);
        });
    }

  }
};
</script>