<template>
  <div class="container mt-4" style="background: #e6d076">
    <h1 class="text-center mb-4">Shopping Cart</h1>

    <form class="row mb-3">
      <input v-model="newProduct.name" placeholder="Product name" class="form-control col" required />
      <input v-model="newProduct.price" type="number" placeholder="Price" class="form-control col" required />
      <input v-model="newProduct.quantity" type="number" placeholder="Quantity" class="form-control col" required />
      <button @click="addProduct(newProduct)" class="btn col">Add Product</button>
    </form>

    <table class="table">
      <thead>
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
          <button @click="removeProduct(item.id)" class="btn btn-sm">Remove</button>
        </td>
      </tr>
      </tbody>
    </table>

    <div class="d-flex justify-content-between mt-4">
      <h3>Cart total: </h3>
      <h3>{{ cartTotal.toFixed(2) }} €</h3>
    </div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  data: () => ({
    api: "http://192.168.55.219:8089/api/cart",
    newProduct: { name: "", price: "", quantity: "" },
    cartItems: [],
    cartTotal: 0,
  }),
  methods: {
    fetchCart() {
      axios
          .all([
            axios.get(`${this.api}/items`).then((res) => (this.cartItems = res.data)),
            axios.get(`${this.api}/total`).then((res) => (this.cartTotal = res.data)),
          ]);
    },
    addProduct() {
      axios.post(`${this.api}/add`, this.newProduct).then(this.fetchCart);
      this.newProduct = {name: "", price: "", quantity: ""};
    },
    removeProduct(id) {
      axios.delete(`${this.api}/remove/${id}`).then(this.fetchCart);
    },
  },
  mounted() {
    this.fetchCart();
  },
};
</script>

<style>
th, td {
  background-color: #aa6f7c !important;
}

th {
  color: cornsilk !important;
}

button {
  border: none;
  border-radius: 8px;
  padding: 10px 20px;
  font-weight: bold;
  background: linear-gradient(to right, #f39c12, #e74c3c);
  color: white;
  cursor: pointer;
  transition: transform 0.2s, background 0.3s;
}

button:hover {
  transform: scale(1.1);
  background: linear-gradient(to right, #e74c3c, #f39c12);
}
</style>