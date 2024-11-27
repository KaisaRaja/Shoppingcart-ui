<template>
  <div class="container mt-4" style="background: #e9b97a">
    <h1 class="text-center mb-4">Shopping Cart</h1>

    <form @submit.prevent="addProdict" class="row mb-3">
    <input v-model="newProduct.name" placeholder="Product name" class="form-control col" required />
      <input v-model="newProduct.price" type="number" placeholder="Price" class="form-control col" required />
      <input v-model="newProduct.quantity" type="number" placeholder="Quantity" class="form-control col" required />
    <button @click="addProduct(newProduct)" class="btn btn-primary col">Add Product</button>
    </form>

    <table class="table">
      <thead style="color: crimson">
      <tr>
        <th>Product</th>
        <th>Quantity</th>
        <th>Price</th>
        <th>Action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in cartItems" :key="item">
        <td>{{ item.name }}</td>
        <td>{{ item.quantity }}</td>
        <td>{{ item.price }} €</td>
        <td>
          <button @click="removeProduct(item.name)" class="btn btn-warning btn-sm">Remove</button>
        </td>
      </tr>
      </tbody>
    </table>


    <div class="d-flex justify-content-between mt-4">
<h3>Cart total: </h3>
    <h3> {{cartTotal.toFixed(2)}} €</h3>
  </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: () => ({
    api: "http://localhost:8089/api/cart",
    newProduct: { name: "", price: 0, quantity: 1 },
    cartItems: [],
    cartTotal: 0,
  }),
  methods: {
    fetchCart() {
      axios.all([
        axios.get(`${this.api}/items`).then(res => (this.cartItems = res.data)),
        axios.get(`${this.api}/total`).then(res => (this.cartTotal = res.data)),
      ]);
    },
  addProduct() {
  axios.post(`${this.api}/add`, this.newProduct).then(this.fetchCart);
  this.newProduct = {name: "", price: 0, quantity: 1};
 },
    removeProduct(name) {
      axios.delete(`${this.api}/remove/${name}`).then(this.fetchCart);
    },
  },
  mounted() {
    this.fetchCart();
  },
};
</script>




<style>

</style>