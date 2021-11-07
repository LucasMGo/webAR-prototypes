<template>
  <v-container class="ma-md-12">
    <v-row justify="start">
      <v-col cols="auto">
        <h1 class="font-weight-light">{{ name }}</h1>
      </v-col>
      <v-col cols="auto">
        <v-btn id="ar-button">View in AR</v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" lg="6" class="font-weight-light">
        <p>
          {{ description }}
        </p>
      </v-col>
    </v-row>
    <v-row no-gutters class="mt-6">
      <v-col cols="12">
        <zimmerpflanze v-if="name === 'Zimmerpflanze'"></zimmerpflanze>
        <vase v-else-if="name === 'Vase'"></vase>
        <tischlampe v-else-if="name === 'Tischlampe'"></tischlampe>
        <auto v-else-if="name === 'Auto'"></auto>
      </v-col>
    </v-row>
    <v-row no-gutters>
      <v-col cols="12" lg="6">
        <em class="credit">{{ credit }}</em>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import products from "../data/products.json";
import Tischlampe from "../components/Tischlampe.vue";
import Vase from "../components/Vase.vue";
import Zimmerpflanze from "../components/Zimmerpflanze.vue";
import Auto from "../components/Auto.vue";
export default {
  name: "Product",
  components: {
    Tischlampe,
    Vase,
    Zimmerpflanze,
    Auto,
  },
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