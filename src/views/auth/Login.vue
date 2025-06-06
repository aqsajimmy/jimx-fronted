<script setup lang="ts">
import axiosInstance from "@/lib/axios";
import { reactive, ref } from "vue";
import type { LoginForm } from "@/types";
import router from "@/router";

const form = reactive<LoginForm>({
  email: "",
  password: "",
});

const errors = ref<{ [key: string]: string[] }>({});

const login = async (payload: LoginForm) => {
  await axiosInstance.get("/sanctum/csrf-cookie", {
    baseURL: "http://localhost:8000",
  });
  try {
    await axiosInstance.post("/login", payload);
    router.push("/dashboard");
  } catch (error: any) {
    errors.value = error.response?.data?.errors || {
      general: ["An error occurred"],
    };
    // console.error(error.response?.data);
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
      <div v-if="errors.email" class="text-red-500 text-sm">
      {{ errors.email[0] }}
    </div>
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
    <button
      cursor="pointer"
      type="submit"
      class="text-white bg-indigo-500 border-0 py-2 px-6 focus:outline-none hover:bg-indigo-600 rounded text-lg focus:shadow-outline cursor-pointer"
    >
      Login
    </button>
    <!-- General error message -->
    <div v-if="errors.general" class="text-red-500 text-sm mt-2">
      {{ errors.general[0] }}
    </div>
  </form>
</template>
