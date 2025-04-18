<script setup lang="ts">
import { ref } from 'vue';

const inputValue = ref('');
const emit = defineEmits(['submit']);

function handleSubmit() {
  if (inputValue.value.trim()) {
    emit('submit', inputValue.value);
    inputValue.value = '';
  }
}
</script>

<template>
  <div class="flex flex-col items-center justify-center gap-5 px-4 py-10">
    <div class="w-full max-w-xl bg-gradient-to-br from-purple-600 to-pink-500 p-1 rounded-2xl shadow-2xl">
      <div class="flex bg-gray-900 rounded-xl overflow-hidden shadow-inner">
        <input
          v-model="inputValue"
          @keyup.enter="handleSubmit"
          placeholder="Digite um usuário GitHub..."
          class="flex-1 px-5 py-4 bg-transparent text-white placeholder-gray-400 focus:outline-none text-lg tracking-wide"
        />
        
        <button
          @click="handleSubmit"
          class="px-6 py-4 bg-gradient-to-r from-purple-500 to-pink-500 hover:from-purple-700 hover:to-pink-700 text-white font-semibold text-sm uppercase tracking-wide transition-all duration-300 flex items-center justify-center"
        >
          <span class="mr-2">Buscar</span>
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 animate-pulse" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
        </button>
      </div>
    </div>

    <p class="text-gray-400 text-sm italic mt-3">
      Exemplos: <span class="text-white">octocat</span>, <span class="text-white">torvalds</span>, <span class="text-white">mojombo</span>
    </p>
  </div>
</template>

<style scoped>
input:focus {
  box-shadow: 0 0 0 3px rgba(168, 85, 247, 0.5);
}

input::placeholder {
  transition: opacity 0.3s ease;
  opacity: 0.6;
}

input:focus::placeholder {
  opacity: 0.3;
}
</style>
