<template>
  <v-container class="ma-md-12">
    <v-row justify="start">
      <v-col cols="auto">
        <h1 class="font-weight-light">
          {{ name }}
          <v-btn class="ml-2" outlined :loading="!viewAR" id="ar-button">
            View in AR
          </v-btn>
        </h1>
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
        <div id="dom-overlay">
          <div id="button-wrapper">
            <v-row no-gutters>
              <v-col cols="auto">
                <v-btn outlined id="increase-button">
                  Size +25%
                  <v-icon large>mdi-plus-box-outline</v-icon>
                </v-btn>
              </v-col>
              <v-col cols="auto">
                <v-btn outlined id="decrease-button">
                  Size -25%
                  <v-icon large>mdi-minus-box-outline</v-icon>
                </v-btn>
              </v-col>
              <v-spacer></v-spacer>
              <v-col>
                <v-btn outlined id="exit-ar-button">Exit AR</v-btn>
              </v-col>
              <v-spacer></v-spacer>
              <v-col cols="auto">
                <v-btn outlined id="turnleft-button">
                  Turn
                  <v-icon large>mdi-arrow-left-top-bold</v-icon>
                </v-btn>
              </v-col>
              <v-col cols="auto">
                <v-btn outlined id="turnright-button">
                  Turn
                  <v-icon large>mdi-arrow-right-top-bold</v-icon>
                </v-btn>
              </v-col>
            </v-row>
          </div>
        </div>
        <zimmerpflanze
          @modelLoaded="viewAR = true"
          v-if="name === 'Zimmerpflanze'"
        ></zimmerpflanze>
        <vase @modelLoaded="viewAR = true" v-else-if="name === 'Vase'"></vase>
        <tischlampe
          @modelLoaded="viewAR = true"
          v-else-if="name === 'Tischlampe'"
        ></tischlampe>
        <auto @modelLoaded="viewAR = true" v-else-if="name === 'Auto'"></auto>
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
/* eslint-disable no-undef */
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
  data: () => ({
    viewAR: false,
  }),
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
  mounted() {
    const buttonWrapper = document.getElementById("button-wrapper");
    buttonWrapper.addEventListener("beforexrselect", (e) => {
      e.preventDefault();
    });
  },
};
</script>

<style scoped>
.credit {
  font-size: 0.8em;
}
#dom-overlay {
  display: none;
}
/* Show the DOM Overlay element while active (:xr-overlay pseudoclass) */
#dom-overlay:xr-overlay {
  display: initial;
}
</style>