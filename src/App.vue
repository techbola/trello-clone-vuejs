<script setup lang="ts"></script>

<template>
  <main class="p-5 font-sans">
    <div class="flex gap-5 py-5 overflow-x-auto">
      <div class="bg-gray-100 p-3 rounded-lg min-w-[250px] flex flex-col">
        <h2 class="mb-2 font-medium">The List</h2>
        <div class="p-2 my-2 bg-white rounded shadow cursor-pointer">
          <span class="text-sm font-medium">Title Card</span>
          <p class="text-xs text-gray-400">Card description</p>
        </div>

        <div class="p-2 my-2 bg-white rounded shadow cursor-pointer">
          <span class="text-sm font-medium">Title Card</span>
          <p class="text-xs text-gray-400">Card description</p>
        </div>

        <div class="p-2 my-2 bg-white rounded shadow cursor-pointer">
          <span class="text-sm font-medium">Title Card</span>
          <p class="text-xs text-gray-400">Card description</p>
        </div>

        <button
          class="w-full p-2 mt-2 text-sm font-medium text-left text-gray-500 bg-transparent rounded hover:bg-white"
        >
          + Add card
        </button>
      </div>
    </div>
  </main>
</template>

<style scoped></style>
