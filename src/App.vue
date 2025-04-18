<script setup lang="ts">
import CardUser from './components/CardUser.vue';
import InputUser from './components/InputUser.vue';
import { ref } from 'vue';

const githubUsername = ref('');

function handleSubmit(username: string) {
  githubUsername.value = username;
}
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-gray-900 to-gray-800 flex flex-col items-center justify-start py-12 px-4 sm:px-6 lg:px-8">
    <!-- Header -->
    <div class="text-center mb-12">
      <h1 class="text-4xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-purple-600 mb-4">
        GitHub Profile Finder
      </h1>
      <p class="text-lg text-gray-300 max-w-md">
        Descubra perfis do GitHub e veja suas estatísticas principais
      </p>
    </div>

    <!-- Input Section -->
    <div class="w-full max-w-md mb-12">
      <InputUser @submit="handleSubmit" />
    </div>

    <!-- Card Section -->
    <div class="w-full max-w-md transform transition-all duration-300 hover:scale-[1.02]">
      <CardUser :username="githubUsername" />
    </div>

    <!-- Footer -->
    <div class="mt-16 text-center text-gray-400 text-sm">
      <p>Feito com ❤️ usando Vue.js e Tailwind CSS</p>
    </div>
  </div>
</template>