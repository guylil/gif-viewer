<template>
  <v-container fluid>
    <v-card class="mx-auto" max-width="350" outlined>
      <div class="flex-column mt-3">
        <div class="d-flex justify-center">
          Please login
        </div>
        <div class="d-flex justify-center mt-3">
          <img
            src="https://callvu.com/wp-content/uploads/2017/12/logo.png"
            height="45px"
            width="150px"
            alt="logo"
          />
        </div>
        <transition name="show">
          <v-text-field
            v-model="apiKey"
            v-if="isLoggedIn"
            class="mt-5 px-5"
            label="Please enter your api key"
            @keydown.enter="login"
          >
            {{ apiKey }}
          </v-text-field>
        </transition>
        <div class="d-flex justify-space-around my-5">
          <v-btn
            outlined
            rounded
            text
            @click="login"
            :class="[isHasKey ? 'cyan white--text' : '']"
          >
            Login
          </v-btn>
          <v-btn outlined rounded text @click="logout">
            Logout
          </v-btn>
        </div>
      </div>
    </v-card>
  </v-container>
</template>

<script>
import router from "@/router";

export default {
  name: "Login",

  data: () => ({
    name: "Login",
    isLoggedIn: false,
    apiKey: ""
  }),
  methods: {
    login() {
      if (!this.isLoggedIn) {
        this.isLoggedIn = true;
      } else {
        localStorage.setItem("apiKey", this.apiKey);
      }
      if (this.apiKey) {
        router.push("/main");
      }
    },
    logout() {
      localStorage.removeItem("apiKey");
      this.isLoggedIn = false;
      this.apiKey = "";
    }
  },
  mounted() {
    this.apiKey = localStorage.getItem("apiKey") || "";
  },
  computed: {
    isHasKey() {
      return this.apiKey.length > 4;
    }
  }
};
</script>
<style>
.show-enter-active,
.show-leave-active {
  transition: opacity 0.5s;
}
.show-enter,
.show-leave-to {
  opacity: 0;
}
</style>
