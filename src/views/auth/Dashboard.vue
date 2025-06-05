<script setup lang="ts">
import axiosInstance from "@/lib/axios";
import { get } from "http";
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
    //clear cookie after logout
    document.cookie = "XSRF-TOKEN=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/";
    document.cookie = "laravel_session=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/";
    window.location.href = "/login";
  } catch (error: any) {
    errors.value = error.response?.data?.errors || ["An error occurred"];
    console.error(error.response?.data);
  }
};
getUser();
</script>
<template>
  <div class="max-w-2xl mx-auto mt-10">
    <h1>Dashboard</h1>
  </div>
  <div v-if="user.name" class="max-w-2xl mx-auto mt-10 flex flex-col items-center bg-white p-6 rounded-lg shadow-md">
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
    <p>You are not logged in.</p>
  </div>
</template>
