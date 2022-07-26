<template>
  <div class="home">
    <h1>{{ message }} Count: {{ products.length }}</h1>
    <h2>New Product</h2>
    Name
    <input type="text" v-model="newProduct.name" />
    Description
    <input type="text" v-model="newProduct.description" />
    Price
    <input type="text" v-model="newProduct.price" />
    Image URL
    <input type="text" v-model="newProduct.image_url" />
    <button v-on:click="createProduct">Create</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h3>{{ product.name }}</h3>
      <div>
        <button v-on:click="showProduct(product)">Info</button>
      </div>
      <h4>Description: {{ product.description }}</h4>
      <p>price: {{ product.price }}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name" style="max-width: 250px" />
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info</h1>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <p>Image_url: {{ currentProduct.image_url }}</p>
        <h1>Update Product</h1>
        <p>
          Name:
          <input v-model="currentProduct.name" type="text" />
        </p>
        <p>
          Description:
          <input v-model="currentProduct.description" type="text" />
        </p>
        <p>
          Price:
          <input v-model="currentProduct.price" type="text" />
        </p>
        <p>
          Image_url:
          <input v-model="currentProduct.image_url" type="text" />
        </p>
        <button v-on:click="updateProduct()">Update</button>
        <button v-on:click="deleteProduct(currentProduct)">Delete</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Simple Mini Capstone:",
      products: [],
      newProduct: {},
      currentProduct: {},
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
      axios
        .post("http://localhost:3000/products", this.newProduct)
        .then((response) => {
          this.products.push(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
    showProduct(product) {
      axios.get(`http://localhost:3000/products/${product.id}`).then((response) => {
        this.currentProduct = response.data;
        this.editProduct = response.data;
        document.querySelector("#product-details").showModal();
      });
    },
    updateProduct() {
      axios
        .patch(`http://localhost:3000/products/${this.currentProduct.id}`, this.currentProduct)
        .then((response) => {
          console.log(response);
          var index = this.products.indexOf(this.currentProduct);
          this.products.splice(index, 1);
          this.products.push(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
    deleteProduct(product) {
      axios
        .delete(`http://localhost:3000/products/${product.id}`)
        .then((response) => {
          console.log(response);
          var index = this.products.indexOf(product);
          this.products.splice(index, 1);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
  },
};
</script>

<style>
.home {
  background-image: url("https://img3.goodfon.com/original/1680x1050/0/2c/art-canarinu-kmes-paren-les.jpg");
}
h4 {
  color: hotpink;
  font: 600;
}
h3 {
  color: green;
}
h1 {
  color: blue;
}
</style>
