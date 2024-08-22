<template>
  <h1>Login Page</h1>
  <v-text-field type="email" v-model="emailAddress" label="email" />
  <v-text-field type="password" v-model="password" label="password" />
  <v-btn @click="login">Submit</v-btn>
</template>

<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";
import router from "@/router";

const emailAddress = ref("");
const password = ref("");

const login = () => {
  axios
    .post("https://api.utrack.solutions/login", {
      headers: {
        accept: "application/json",
        "Content-Type": "application/json",
      },
      emailAddress: emailAddress.value,
      password: password.value,
    })
    .then(function (response) {
      console.log(response);
      localStorage.setItem("loginToken", response.data.accessToken);
      router.push("/projectPage");
    })
    .catch(function (error) {
      console.log(emailAddress.value, password.value);
      console.log(error);
    });
};
</script>
