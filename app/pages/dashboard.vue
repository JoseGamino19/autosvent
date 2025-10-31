<template>
  <div class="min-h-screen flex flex-col items-center justify-center bg-blue-100">
    <h1 class="text-3xl font-bold text-blue-800 mb-4">Bienvenido a la AutosVent</h1>
    <p>Has iniciado sesión correctamente.</p>
    <button @click="logout" class="bg-red-600 text-white px-3 py-1 rounded m-4">Cerrar sesión</button>
  </div>
</template>

<script setup >
import { ref } from 'vue'

const email = ref('')
const password = ref('')
const errorMsg = ref('')
const loading = ref(false)

const { $supabase } = useNuxtApp()
const router = useRouter()
const logout = async () => {
  await $supabase.auth.signOut()
  console.log('saliste')
  router.push('/login')
}
</script>