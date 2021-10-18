<template>
  <v-app>
    <v-app-bar color="blue darken-2" dark app>
      <v-app-bar-nav-icon @click.stop="showDrawer = !showDrawer"></v-app-bar-nav-icon>
      <v-spacer />
      <v-toolbar-title>WebAR Onlineshop</v-toolbar-title>
      <v-spacer />
    </v-app-bar>

    <v-navigation-drawer v-model="showDrawer" app temporary>
      <v-list nav>
        <v-list-item-group>
          <v-list-item router :to="entry.path" :key="i" v-for="(entry, i) in entries">
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
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    showDrawer: false,
    entries: [
      { icon: "mdi-home", title: "Home", path: "/" },
      { icon: "mdi-basket", title: "Product", path: "/product" },
      { icon: "mdi-alpha-a-box", title: "About", path: "/about" },
    ],
  }),
};
</script>
