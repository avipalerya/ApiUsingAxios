<template>
  <div id="app">
    <!-- <products-main /> -->
    <users-comp />
  </div>
</template>

<script>
import UsersComp from "./components/UsersComp.vue";
// import ProductsMain from './components/Products-main.vue'

export default {
  name: "App",
  components: {
    UsersComp,
    //  ProductsMain
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
