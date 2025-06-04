<script setup lang="ts">

</script>

<template>
  <section class="text-gray-600 body-font bg-gray-100">
    <div class="container mx-auto px-5 py-24">
      <div class="flex flex-col text-center w-full mb-20">
        <h1 class="text-2xl mb-4 font-medium text-gray-900">Login</h1>
        <p class="mb-8 leading-relaxed">
          Please enter your email and password to login.
        </p>
      </div>
      <div class="flex flex-wrap -m-4">
        <div class="p-4 sm:w-1/2 lg:w-1/3">
          <div class="text-left">
            <label for="email" class="block text-sm font-medium text-gray-700"
              >Email</label
            >
            <input
              type="email"
              id="email"
              name="email"
              class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
            />
          </div>
        </div>
        <div class="p-4 sm:w-1/2 lg:w-1/3">
          <div class="text-left">
            <label
              for="password"
              class="block text-sm font-medium text-gray-700"
              >Password</label
            >
            <input
              type="password"
              id="password"
              name="password"
              class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
            />
          </div>
        </div>
        <div class="p-4 sm:w-1/2 lg:w-1/3">
          <button
            type="submit"
            class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          >
            Login
          </button>
        </div>
      </div>
    </div>
  </section>
</template>
