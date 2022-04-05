<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>New Product</h2>
    Name
    <input type="text" v-model="productName" />
    Description
    <input type="text" v-model="productDescription" />
    Price
    <input type="text" v-model="productPrice" />
    Image URL
    <input type="text" v-model="productImage_Url" />
    <button v-on:click="createProduct">Create</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h3>{{ product.name }}</h3>
      <h4>Description: {{ product.description }}</h4>
      <p>price: {{ product.price }}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name" />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "simple mini capstone",
      message2: "bleh",
      greeting: "ayo rocko!!",
      products: [],
      productName: "",
      productDescription: "",
      productPrice: 0,
      productImage_Url: "",
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts() {
      axios.get("http://localhost:3000/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProduct() {
      var params = {
        name: this.productName,
        description: this.productDescription,
        price: this.productPrice,
        image_url: this.productImage_Url,
      };
      axios
        .post("http://localhost:3000/products", params)
        .then((response) => {
          this.products.push(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
  },
};
</script>

<style>
h4 {
  color: hotpink;
  font: 600;
}
h3 {
  color: green;
}
</style>
