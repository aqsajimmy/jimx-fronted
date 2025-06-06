<script setup lang="ts">
import axiosInstance from "@/lib/axios";
import router from "@/router";
import { onMounted, onUnmounted, reactive, ref } from "vue";
import type { User } from "@/types";

const user = ref<User | null>(null);

const errors = reactive({
  value: [] as string[],
});

const getUser = async () => {
  try {
    const response = await axiosInstance.get("/user");
    user.value = response.data;
  } catch (error: any) {
    errors.value = error.response?.data?.errors || ["An error occurred"];
    console.error(error.response?.data);
  }
};

const logout = async () => {
  try {
    await axiosInstance.post("/logout");
    user.value = null;
    router.push("/login");
  } catch (error: any) {
    errors.value = error.response?.data?.errors || ["An error occurred"];
    console.error(error.response?.data);
  }
};
onMounted(() => {
  getUser();
});
</script>
<template>
  <div class="max-w-2xl mx-auto mt-10">
    <h1>Dashboard</h1>
  </div>
  <div v-if="user && user.name === ''">

  </div>
  <div
    v-if="user && user.name"
    class="max-w-2xl mx-auto mt-10 flex flex-col items-center bg-white p-6 rounded-lg shadow-md"
  >
    <h2 class="text-2xl font-bold mb-4">User Information</h2>
    <p>Welcome, {{ user.name }}!</p>
    <p>Email: {{ user.email }}</p>
    <button
      @click="logout"
      class="text-white bg-slate-800 border-0 py-2 px-6 focus:outline-none hover:bg-slate-700 rounded text-lg focus:shadow-outline cursor-pointer mt-4"
    >
      Logout
    </button>
  </div>
  <div v-else>
    <div class="animate-pulse flex flex-col items-center">
      <div class="h-6 bg-gray-300 rounded w-1/3 mb-4"></div>
      <div class="h-4 bg-gray-300 rounded w-2/3"></div>
      <div class="h-4 bg-gray-300 rounded w-1/2 mt-2"></div>
      <div class="h-4 bg-gray-300 rounded w-1/4 mt-2"></div>
    </div>
  </div>
</template>
