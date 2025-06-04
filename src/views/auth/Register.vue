<script setup lang="ts">
import axios from "axios";

axios.defaults.withCredentials = true;
axios.defaults.withXSRFToken = true;
axios.defaults.baseURL = "http://127.0.0.1:8000";

const register = async () => {
  await axios.get("/sanctum/csrf-cookie");
  const response = await axios.post("/api/register", {
    name: "newuser",
    email: "newuser@example.com",
    password: "password",
    password_confirmation: "password",
  });
  try {
    console.log(response.data);
  } catch (error) {
    console.log(error);
  }
};
register();
</script>

<template>
  <div>
    <h1>Register</h1>
  </div>
</template>
