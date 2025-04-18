<script setup lang="ts">
import { ref, watch } from 'vue';

interface GitHubUser {
  name: string;
  bio: string;
  avatar_url: string;
  html_url: string;
  public_repos: number;
  followers: number;
  following: number;
}

const props = defineProps<{
  username: string;
}>();

const user = ref<GitHubUser | null>(null);
const isLoading = ref(false);
const error = ref<string | null>(null);

const fetchGithubUser = async () => {
  if (!props.username) {
    user.value = null;
    return;
  }

  isLoading.value = true;
  error.value = null;

  try {
    const response = await fetch(`https://api.github.com/users/${props.username}`);
    
    if (!response.ok) {
      throw new Error('Usuário não encontrado');
    }

    user.value = await response.json();
  } catch (err) {
    error.value = err instanceof Error ? err.message : 'Erro ao buscar usuário';
    user.value = null;
  } finally {
    isLoading.value = false;
  }
};

watch(() => props.username, fetchGithubUser, { immediate: true });
</script>

<template>
  <div class="max-w-md mx-auto my-8">
    <div v-if="isLoading" class="flex flex-col items-center justify-center p-8 space-y-4">
      <div class="w-12 h-12 border-4 border-gray-200 border-t-blue-500 rounded-full animate-spin"></div>
      <p class="text-gray-600 font-medium">Buscando informações...</p>
    </div>
    
    <div v-else-if="error" class="bg-red-50 rounded-xl p-6 text-center">
      <div class="text-4xl mb-3">⚠️</div>
      <h3 class="text-xl font-bold text-red-800 mb-1">Ocorreu um erro</h3>
      <p class="text-red-600 mb-4">{{ error }}</p>
      <button 
        @click="fetchGithubUser" 
        class="px-4 py-2 bg-gray-800 text-white rounded-lg hover:bg-gray-700 transition-colors"
      >
        Tentar novamente
      </button>
    </div>
    
    <div v-else-if="user" class="bg-white rounded-xl shadow-lg overflow-hidden border border-gray-100">
      <div class="bg-gradient-to-r from-gray-700 to-gray-900 p-6">
        <div class="flex items-center space-x-4">
          <img 
            :src="user.avatar_url" 
            alt="Avatar"
            class="w-16 h-16 rounded-full border-4 border-white shadow"
          />
          <div>
            <h2 class="text-xl font-bold text-white">{{ user.name || props.username }}</h2>
            <a 
              :href="user.html_url" 
              target="_blank" 
              class="text-sm text-blue-100 hover:text-white hover:underline"
            >
              @{{ props.username }}
            </a>
          </div>
        </div>
      </div>
      
      <div class="p-6">
        <p class="text-gray-700 mb-6">{{ user.bio || 'Este usuário não possui biografia.' }}</p>
        
        <div class="grid grid-cols-3 gap-4 text-center">
          <div class="bg-gray-50 p-3 rounded-lg">
            <p class="text-2xl font-bold text-gray-800">{{ user.public_repos }}</p>
            <p class="text-sm text-gray-500">Repositórios</p>
          </div>
          <div class="bg-gray-50 p-3 rounded-lg">
            <p class="text-2xl font-bold text-gray-800">{{ user.followers }}</p>
            <p class="text-sm text-gray-500">Seguidores</p>
          </div>
          <div class="bg-gray-50 p-3 rounded-lg">
            <p class="text-2xl font-bold text-gray-800">{{ user.following }}</p>
            <p class="text-sm text-gray-500">Seguindo</p>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="bg-gray-50 rounded-xl p-8 text-center">
      <div class="text-5xl mb-4 text-gray-300">🔍</div>
      <h3 class="text-xl font-bold text-gray-700 mb-2">Buscar usuário do GitHub</h3>
      <p class="text-gray-500">Digite um nome de usuário para visualizar o perfil</p>
    </div>
  </div>
</template>