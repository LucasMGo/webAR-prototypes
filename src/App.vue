<template>
  <v-app>
    <v-app-bar color="blue darken-2" dark app>
      <v-app-bar-nav-icon
        @click.stop="showDrawer = !showDrawer"
      ></v-app-bar-nav-icon>
      <v-toolbar-title>WebAR Onlineshop</v-toolbar-title>
      <v-spacer />
    </v-app-bar>

    <v-navigation-drawer v-model="showDrawer" app temporary>
      <v-list nav>
        <v-list-item-group>
          <v-list-item
            router
            :to="entry.path"
            :key="i"
            v-for="(entry, i) in entries"
          >
            <v-list-item-icon>
              <v-icon>{{ entry.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>{{ entry.title }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <router-view :key="$route.fullPath" />
    </v-main>
  </v-app>
</template>

<script>
/* eslint-disable no-undef */
import products from "./data/products.json";
export default {
  name: "App",

  data: () => ({
    showDrawer: false,
  }),
  computed: {
    productEntries() {
      const productEntries = [];
      for (const [key, value] of Object.entries(products)) {
        productEntries.push({
          icon: "mdi-basket",
          title: value.name,
          path: `/product/${key}`,
        });
      }
      return productEntries;
    },
    entries() {
      const entries = [
        { icon: "mdi-home", title: "Home", path: "/" },
        ...this.productEntries,
        { icon: "mdi-alpha-a-box", title: "About", path: "/about" },
      ];
      return entries;
    },
  },
  mounted() {
    AFRAME.registerComponent("hide-on-hit-test-start", {
      init: function () {
        const model = this.el;
        model.sceneEl.addEventListener("ar-hit-test-start", () => {
          console.log(
            "AR Hit test started. Model not visible until hit point selected"
          );
          model.object3D.visible = false;
        });
      },
    });

    AFRAME.registerComponent("increase-on-click", {
      init: function () {
        const model = this.el;
        const increaseButton = document.getElementById("increase-button");
        increaseButton.addEventListener("click", () => {
          const mesh = model.getObject3D("mesh");
          const currentMeshScale = mesh.scale;
          mesh.scale.set(
            currentMeshScale.x * 1.25,
            currentMeshScale.y * 1.25,
            currentMeshScale.z * 1.25
          );
        });
      },
    });

    AFRAME.registerComponent("decrease-on-click", {
      init: function () {
        const model = this.el;
        const decreaseButton = document.getElementById("decrease-button");
        decreaseButton.addEventListener("click", () => {
          const mesh = model.getObject3D("mesh");
          const currentMeshScale = mesh.scale;
          mesh.scale.set(
            currentMeshScale.x / 1.25,
            currentMeshScale.y / 1.25,
            currentMeshScale.z / 1.25
          );
        });
      },
    });

    AFRAME.registerComponent("turn-left-on-click", {
      init: function () {
        const model = this.el;
        const turnleftButton = document.getElementById("turnleft-button");
        turnleftButton.addEventListener("click", () => {
          const mesh = model.getObject3D("mesh");
          mesh.rotateY(-Math.PI / 2);
        });
      },
    });

    AFRAME.registerComponent("turn-right-on-click", {
      init: function () {
        const model = this.el;
        const turnrightButton = document.getElementById("turnright-button");
        turnrightButton.addEventListener("click", () => {
          const mesh = model.getObject3D("mesh");
          mesh.rotateY(Math.PI / 2);
        });
      },
    });

    AFRAME.registerComponent("exit-ar-on-click", {
      init: function () {
        const exitARButton = document.getElementById("exit-ar-button");
        exitARButton.addEventListener("click", () => {
          this.el.sceneEl.exitVR();
        });
      },
    });
  },
};
</script>
