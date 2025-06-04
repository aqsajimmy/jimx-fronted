<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";

axios.defaults.baseURL = "http://localhost:8000";
axios.defaults.withCredentials = true;
axios.defaults.withXSRFToken = true;

const name = ref("");
const email = ref("");
const password = ref("");
const password_confirmation = ref("");

const errors = ref([]);

const register = async () => {
  try {
    await axios.get("/sanctum/csrf-cookie");
    const response = await axios.post("/api/register", {
      name: name.value,
      email: email.value,
      password: password.value,
      password_confirmation: password_confirmation.value,
    });
    console.log(response.data);
    // Handle successful registration
  } catch (error: any) {
    errors.value = error.response.data.errors;
    console.error(error.response.data);
  }
};
</script>

<template>
  <div class="bg-blue-500 text-white p-4 rounded">
    <h1 class="text-2xl font-bold">Register</h1>
  </div>
  <form @submit.prevent="register">
    <div>
      <label for="name">Name</label>
      <input type="text" v-model="name" />
    </div>
    <div>
      <label for="email">Email</label>
      <input type="email" v-model="email" />
    </div>
    <div>
      <label for="password">Password</label>
      <input type="password" v-model="password" />
    </div>
    <div>
      <label for="password_confirmation">Password Confirmation</label>
      <input type="password" v-model="password_confirmation" />
    </div>
    <button type="submit">Register</button>
  </form>
</template>
