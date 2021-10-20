<template>
  <v-container class="ma-md-12">
    <v-row
      ><v-col cols="12"
        ><h1 class="font-weight-light">{{ name }}</h1></v-col
      ></v-row
    >
    <v-row>
      <v-col cols="12" lg="6" class="font-weight-light">
        <p>
          {{ description }}
        </p>
      </v-col>
    </v-row>
    <v-row no-gutters class="mt-6">
      <v-col cols="12" lg="6">
        <v-img
          :aspect-ratio="1"
          src="https://i.stack.imgur.com/y9DpT.jpg"
        ></v-img> </v-col
    ></v-row>
    <v-row no-gutters>
      <v-col cols="12" lg="6">
        <em class="credit">{{ credit }}</em>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import products from "../data/products.json";
export default {
  name: "Product",
  computed: {
    productInfo() {
      const thisProduct = products[this.$router.currentRoute.params.id];
      return thisProduct;
    },
    description() {
      return this.productInfo.description;
    },
    name() {
      return this.productInfo.name;
    },
    credit() {
      return this.productInfo.credit;
    },
  },
};
</script>

<style scoped>
.credit {
  font-size: 0.8em;
}
</style>