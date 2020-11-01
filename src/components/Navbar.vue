<template>
  <div>
    <v-navigation-drawer v-model="drawer" app temporary>
      <v-list dense> </v-list>
    </v-navigation-drawer>
    <v-app-bar app light>
      <v-toolbar-title>Weather API</v-toolbar-title>

      <v-spacer></v-spacer>
      <v-app-bar-nav-icon
        class="hidden-md-and-up"
        @click.stop="drawer = !drawer"
      />
    </v-app-bar>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "Navbar",
  data: () => ({
    drawer: false
  })
});
</script>
<style>
nav.v-navigation-drawer {
  background: #0d4077 !important;
}

nav.v-navigation-drawer.v-navigation-drawer--fixed.v-navigation-drawer--is-mobile.v-navigation-drawer--open.v-navigation-drawer--temporary.theme--light {
  background: #0d4077 !important;
  color: white;
}
</style>
