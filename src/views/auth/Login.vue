<script setup lang="ts">
import axiosInstance from "@/lib/axios";
import { reactive, ref } from "vue";

interface LoginForm {
  email: string;
  password: string;
}

const form = reactive<LoginForm>({
  email: "",
  password: "",
});

const errors = ref<string[]>([]);

const login = async (payload: LoginForm) => {
  try {
    await axiosInstance.get("/sanctum/csrf-cookie", {
      baseURL: "http://localhost:8000",
    });
    const response = await axiosInstance.post("/login", payload);
    console.log(response.data); // Handle successful login, e.g., redirect or store user data
    // window.location.href = "/dashboard";
  } catch (error: any) {
    errors.value = error.response?.data?.errors || ["An error occurred"];
    console.error(error.response?.data);
  }
};
</script>

<template>
  <form @submit.prevent="login(form)" class="max-w-md mx-auto mt-10">
    <div class="mb-4">
      <label for="email" class="leading-7 text-sm text-gray-600">Email:</label>
      <input
        type="email"
        id="email"
        v-model="form.email"
        class="w-full bg-gray-100 rounded border bg-opacity-50 border-gray-300 focus:ring-2 focus:ring-indigo-200 focus:bg-transparent focus:border-indigo-500 text-base outline-none text-gray-700 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out"
      />
    </div>
    <div class="mb-4">
      <label for="password" class="leading-7 text-sm text-gray-600"
        >Password:</label
      >
      <input
        type="password"
        id="password"
        v-model="form.password"
        class="w-full bg-gray-100 rounded border bg-opacity-50 border-gray-300 focus:ring-2 focus:ring-indigo-200 focus:bg-transparent focus:border-indigo-500 text-base outline-none text-gray-700 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out"
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
      cursor="pointer"
      type="submit"
      class="text-white bg-indigo-500 border-0 py-2 px-6 focus:outline-none hover:bg-indigo-600 rounded text-lg focus:shadow-outline cursor-pointer"
    >
      Login
    </button>
  </form>
</template>
