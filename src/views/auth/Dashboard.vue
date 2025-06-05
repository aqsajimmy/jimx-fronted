<script setup lang="ts">
import axiosInstance from "@/lib/axios";
import { reactive, ref } from "vue";

interface DashboardData {
  // Define the structure of your dashboard data here
}
const dashboardData = reactive<DashboardData>({
  // Initialize your dashboard data
});
const errors = ref<string[]>([]);

const user = async () => {
  try {
    await axiosInstance.get("/sanctum/csrf-cookie", {
      baseURL: "http://localhost:8000",
    });
    const response = await axiosInstance.get("/user");
    console.log(response.data); // Handle successful user data retrieval
  } catch (error: any) {
    errors.value = error.response?.data?.errors || ["An error occurred"];
    console.error(error.response?.data);
  }
};

user();
</script>
<template>
  <div>
    <h1>Dashboard</h1>
  </div>
</template>
