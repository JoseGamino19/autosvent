<script setup lang="ts">
import { ref } from 'vue'
const { $supabase } = useNuxtApp()  
const router = useRouter()

const email = ref('')
const password = ref('')
const errorMsg = ref('')
const loading = ref(false)


const login = async () => {
  errorMsg.value = ''
  loading.value = true

  const { data, error } = await $supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  })

  loading.value = false

  if (error) {
    errorMsg.value = error.message
  } else {
    console.log('✅ Entraste correctamente')
    router.push('/home')
  }
}

</script>


<template>
  <div class="min-h-screen flex items-center justify-center bg-blue-100">
    <div class="bg-white shadow-lg rounded-lg p-8 w-140">
      <div class="flex flex-col justify-center items-center text-blue-600">
        <h2 class="text-2xl font-bold mb-4 text-center">Iniciar Sesión</h2>
        <svg xmlns="http://www.w3.org/2000/svg" width="110" height="110" viewBox="0 0 512 512"><path fill="currentColor" fill-rule="evenodd" d="M256 42.667A213.333 213.333 0 0 1 469.334 256c0 117.821-95.513 213.334-213.334 213.334c-117.82 0-213.333-95.513-213.333-213.334C42.667 138.18 138.18 42.667 256 42.667m21.334 234.667h-42.667c-52.815 0-98.158 31.987-117.715 77.648c30.944 43.391 81.692 71.685 139.048 71.685s108.104-28.294 139.049-71.688c-19.557-45.658-64.9-77.645-117.715-77.645M256 106.667c-35.346 0-64 28.654-64 64s28.654 64 64 64s64-28.654 64-64s-28.653-64-64-64"/></svg>
      </div>
      <form @submit.prevent="login" class="flex flex-col gap-4">
        <input
          v-model="email"
          type="email"
          placeholder="Correo"
          class="border p-2 rounded focus:outline-none focus:ring-2 focus:ring-blue-400"
          required
        />
        <input
          v-model="password"
          type="password"
          placeholder="Contraseña"
          class="border p-2 rounded focus:outline-none focus:ring-2 focus:ring-blue-400"
          required
        />

        <button
          type="submit"
          class="bg-blue-700 text-white py-2 rounded hover:bg-blue-800 transition"
          :disabled="loading"
        >
          {{ loading ? 'Ingresando...' : 'Entrar' }}
        </button>

        <p v-if="errorMsg" class="text-red-600 text-sm text-center">{{ errorMsg }}</p>
      </form>
      <slot />
    </div>
  </div>
</template>

<style scoped>

input {
  width: 100%;
  padding: 10px;
  margin: 5px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
}


button:disabled {
  background-color: #ccc;
}

.error {
  color: red;
  margin-top: 10px;
}
</style>
