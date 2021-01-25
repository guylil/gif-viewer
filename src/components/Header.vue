<template>
  <v-container>
    <v-app-bar app color="cyan" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2 pointer"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
          @click="goToRoot"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn href="https://github.com/guylil" target="_blank" text>
        <span class="mr-2">Visit My Github</span>
        <v-icon>mdi-github</v-icon>
      </v-btn>
    </v-app-bar>
  </v-container>
</template>

<script>
import router from "@/router";

export default {
  name: "Header",

  data: () => ({
    ecosystem: []
  }),
  methods: {
    goToRoot() {
      if (router.history.current.fullPath !== "/") router.push("/");
    }
  }
};
</script>

<style>
.pointer {
  cursor: pointer;
}
</style>
