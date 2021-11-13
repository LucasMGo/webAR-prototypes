<template>
  <div>
    <div v-if="arMode">
      <v-row no-gutters>
        <v-col>
          <iframe
            :src="modelPath"
            :style="{
              width: '100%',
              height: $vuetify.breakpoint.xl ? '85vh' : '80vh',
            }"
          ></iframe>
        </v-col>
      </v-row>
      <v-row no-gutters>
        <v-col class="mx-2 font-weight-light">
          <p>
            {{ credit }}
          </p>
        </v-col>
      </v-row>
    </div>
    <v-container v-else class="ma-md-12">
      <v-row>
        <v-col cols="12">
          <h1 class="font-weight-light">{{ name }}</h1>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" lg="6" class="font-weight-light">
          <p>
            {{ description }}
          </p>
        </v-col>
      </v-row>
      <v-row no-gutters>
        <v-col>
          <v-btn @click="activeAR()">View in AR</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import products from "../data/products.json";
export default {
  name: "Product",
  components: {},
  data: () => ({
    arMode: false,
  }),
  methods: {
    activeAR() {
      this.arMode = true;
    },
  },
  computed: {
    modelPath() {
      switch (this.name) {
        case "Tischlampe":
          return "/tischlampe.html";
        case "Vase":
          return "/vase.html";
        case "Auto":
          return "/auto.html";
        case "Zimmerpflanze":
          return "/zimmerpflanze.html";
        default:
          return "";
      }
    },
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