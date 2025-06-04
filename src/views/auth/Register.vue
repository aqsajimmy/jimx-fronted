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
  <form @submit.prevent="register" class="max-w-md mx-auto mt-10">
    <div class="mb-4">
      <label for="name" class="block text-gray-700 text-sm font-bold mb-2"
        >Name:</label
      >
      <input
        type="text"
        id="name"
        v-model="name"
        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
      />
    </div>
    <div class="mb-4">
      <label for="email" class="block text-gray-700 text-sm font-bold mb-2"
        >Email:</label
      >
      <input
        type="email"
        id="email"
        v-model="email"
        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
      />
    </div>
    <div class="mb-4">
      <label for="password" class="block text-gray-700 text-sm font-bold mb-2"
        >Password:</label
      >
      <input
        type="password"
        id="password"
        v-model="password"
        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
      />
    </div>
    <div class="mb-4">
      <label
        for="password_confirmation"
        class="block text-gray-700 text-sm font-bold mb-2"
        >Confirm Password:</label
      >
      <input
        type="password"
        id="password_confirmation"
        v-model="password_confirmation"
        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
      />
    </div>
    <div v-if="errors.length > 0" class="mb-4">
      <ul>
        <li v-for="error in errors" :key="error" class="text-red-500 text-sm">
          {{ error }}
        </li>
      </ul>
    </div>
    <button
      type="submit"
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
    >
      Register
    </button>
  </form>
</template>
