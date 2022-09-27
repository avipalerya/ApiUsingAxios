<template>
  <div>
    <b-table-simple responsive>
      <b-thead>
        <b-tr>
          <b-th>ID </b-th>
          <b-th>Product Name </b-th>
          <b-th>Product Price</b-th>
          <b-th>Product Description </b-th>
          <b-th>Product Image </b-th>
          <b-th>
            <b-button variant="warning" @click="openAddModal">ADD❤️</b-button>
          </b-th>
        </b-tr>
      </b-thead>
      <b-tbody>
        <b-tr v-for="product in products" :key="product._id">
          <b-td>{{ product._id }}</b-td>
          <b-td>{{ product.productName }}</b-td>
          <b-td>{{ product.productPrice }}</b-td>
          <b-td>{{ product.productDescription }}</b-td>
          <b-td>
            <img :src="product.productImageURL" width="220px" height="160px" />
          </b-td>
          <b-td>
            <b-button
              variant="primary"
              class="mx-2"
              @click="getSelectedProduct(product)"
              >EDIT</b-button
            >
            <b-button variant="danger" @click="deleteSelectedProd(product._id)"
              >DELETE</b-button
            ></b-td
          >
        </b-tr>
      </b-tbody>
    </b-table-simple>

    <!-- add product component -->
    <add-product
      :showHideAddModal="showHideAddModal"
      @closeAddModal="closeAddModal"
      @fetchProducts="fetchProducts"
    />

    <!-- edit product component -->
    <edit-product
      :selectedProduct="selectedProduct"
      :showHideEditModal="showHideEditModal"
      @closeEditModal="closeEditModal"
      @fetchProducts="fetchProducts"
    />
  </div>
</template>

<script>
// import axios from "axios";
import HTTP from "../axiosConfig";
import AddProduct from "./Add-product.vue";
import EditProduct from "./Edit-product.vue";
export default {
  components: { AddProduct, EditProduct },
  data() {
    return {
      products: [],
      selectedProduct: {},
      showHideAddModal: false,
      showHideEditModal: false,
    };
  },

  methods: {
    async fetchProducts() {
      try {
        let response = await HTTP.get("/api/products");
        if (response.data.error) {
          let errMsg = response.data.message;
          console.log("errMsg", errMsg);
        } else {
          let resProducts = response.data.products;
          this.products = resProducts;
          //  console.log("this.products",this.products);
          console.log("products", response.data.products);
        }
      } catch (err) {
        console.log("err", err);
      }
    },

    // open add modal
    openAddModal() {
      this.showHideAddModal = true;
    },
    //close add modal
    closeAddModal() {
      this.showHideAddModal = false;
    },
    // open edit modal
    openEditModal() {
      this.showHideEditModal = true;
    },
    //close edit modal
    closeEditModal() {
      this.showHideEditModal = false;
    },
    //getting the selected product on click of edit
    getSelectedProduct(prod) {
      this.selectedProduct = prod;
      this.openEditModal();
      console.log("selected prod", prod);
    },
    // delete the product
    async deleteSelectedProd(ID) {
      try {
        let response = await HTTP.delete(`/api/products/${ID}`);
        if (response.data.error) {
          let errMsg = response.data.message;
          console.log("errMsg", errMsg);
        } else {
          let resProducts = response.data.products;
          this.products = resProducts;
          //   console.log("this.products",this.products);
          //   console.log("products", response.data.products);
          this.fetchProducts();
        }
      } catch (err) {
        console.log("err", err);
      }
    },
  },
  mounted() {
    this.fetchProducts();
  },
};
</script>

<style>
</style>