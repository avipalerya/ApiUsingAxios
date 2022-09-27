<template>
  <b-modal
    v-model="modalShow"
    @close="handleClose"
    @cancel="handleClose"
    @ok="handleSubmit"
    title="Edit Product"
  >
    <b-form>
      <b-form-input
        placeholder="Enter product name"
        type="text"
        v-model="singleProduct.productName"
        class="mb-2"
      ></b-form-input>

      <b-form-input
        placeholder="Enter product price"
        type="number"
        v-model="singleProduct.productPrice"
        class="mb-2"
      ></b-form-input>

      <b-form-input
        placeholder="Enter product Description"
        type="text"
        v-model="singleProduct.productDescription"
        class="mb-2"
      ></b-form-input>

      <b-form-input
        placeholder="Enter product Image"
        type="text"
        v-model="singleProduct.productImageURL"
      ></b-form-input>
    </b-form>
  </b-modal>
</template>

<script>
import axios from "axios";
export default {
  props: ["selectedProduct", "showHideEditModal"],
  emits: ["closeEditModal", "fetchProducts"],

  data() {
    return {
      singleProduct: {
        productName: "",
        productPrice: "",
        productDescription: "",
        productImageURL: "",
      },
      modalShow: false,
    };
  },
  methods: {
    handleClose() {
      this.$emit("closeEditModal");
    },
    async handleSubmit() {
      try {
        let response = await axios.put(
          `https://ty-shop.herokuapp.com/api/products/${this.singleProduct._id}`,
          this.singleProduct
        );
        if (response.data.error) {
          let errMsg = response.data.message;
          alert(errMsg);
        } else {
          let resProducts = response.data.products;
          this.products = resProducts;
          //   console.log("products", response.data.products);
          alert(response.data.message);
          this.$emit("fetchProducts");
          this.$emit("closeEditModal")
        }
      } catch (err) {
        console.log("err", err);
      }
    },
  },
  // to check if the props is changed or not from parent
  watch: {
    showHideEditModal() {
      this.modalShow = this.$props.showHideEditModal;
    },
    selectedProduct() {
      this.singleProduct = this.$props.selectedProduct;
    },
  },
};
</script>

<style>
</style>