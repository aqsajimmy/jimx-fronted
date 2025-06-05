<script setup lang="ts">
import axiosInstance from "@/lib/axios";
import { onMounted, onUnmounted, reactive, ref } from "vue";

// interface DashboardData {
//   // Define the structure of your dashboard data here
// }
// const dashboardData = reactive<DashboardData>({
//   // Initialize your dashboard data
// });

const errors = ref<string[]>([]);
const user = ref({
  name: "",
  email: "",
});

const getUser = async () => {
  try {
    await axiosInstance.get("/sanctum/csrf-cookie", {
      baseURL: "http://localhost:8000",
    });
    const response = await axiosInstance.get("/user");
    user.value = response.data;
  } catch (error: any) {
    errors.value = error.response?.data?.errors || ["An error occurred"];
    console.error(error.response?.data);
  }
};

const logout = async () => {
  try {
    await axiosInstance.get("/sanctum/csrf-cookie", {
      baseURL: "http://localhost:8000",
    });
    const response = await axiosInstance.post("/logout");
    console.log(response.data); // Handle successful logout
    user.value = {
      name: "",
      email: "",
    };
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
  <div v-if="user.name">
    <p>Welcome, {{ user.name }}!</p>
    <p>Email: {{ user.email }}</p>
    <button
      @click="logout"
      class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
    >
      Logout
    </button>
  </div>
  <div v-else>
    <p>You are not logged in.</p>
  </div>
</template>
