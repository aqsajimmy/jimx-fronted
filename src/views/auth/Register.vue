<script setup lang="ts">
import axiosInstance from "@/lib/axios";
import { reactive, ref } from "vue";

interface RegisterForm {
  name: string;
  email: string;
  password: string;
  password_confirmation: string;
}

const form = reactive<RegisterForm>({
  name: "",
  email: "",
  password: "",
  password_confirmation: "",
});

const errors = ref<string[]>([]);

const register = async (payload: RegisterForm) => {
  await axiosInstance.get("/sanctum/csrf-cookie", {
    baseURL: "http://localhost:8000",
  });
  try {
    const response = await axiosInstance.post("/register", payload);
    console.log(response.data);
  } catch (error) {
    errors.value = error.response?.data?.errors || ["An error occurred"];
    console.error(error.response?.data);
  }
};
</script>

<template>
  <form @submit.prevent="register(form)" class="max-w-md mx-auto mt-10">
    <div class="mb-4">
      <label for="name" class="block text-gray-700 text-sm font-bold mb-2"
        >Name:</label
      >
      <input
        type="text"
        id="name"
        v-model="form.name"
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
        v-model="form.email"
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
        v-model="form.password"
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
        v-model="form.password_confirmation"
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
