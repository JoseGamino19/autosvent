<template>
  <nav class="bg-gray-900 text-white px-6 py-4 flex justify-between items-center shadow-lg">
    <!-- Logo -->
    <NuxtLink to="/home" class="text-2xl font-bold flex items-center space-x-2 hover:text-blue-400">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
        <path  fill="currentColor" d="m16 6l3 4h2c1.11 0 2 .89 2 2v3h-2a3 3 0 0 1-3 3a3 3 0 0 1-3-3H9a3 3 0 0 1-3 3a3 3 0 0 1-3-3H1v-3c0-1.11.89-2 2-2l3-4zm-5.5 1.5H6.75L4.86 10h5.64zm1.5 0V10h5.14l-1.89-2.5zm-6 6A1.5 1.5 0 0 0 4.5 15A1.5 1.5 0 0 0 6 16.5A1.5 1.5 0 0 0 7.5 15A1.5 1.5 0 0 0 6 13.5m12 0a1.5 1.5 0 0 0-1.5 1.5a1.5 1.5 0 0 0 1.5 1.5a1.5 1.5 0 0 0 1.5-1.5a1.5 1.5 0 0 0-1.5-1.5"/></svg>
      <span class="">AutosVent</span>
    </NuxtLink>

    <!-- Menú principal -->
    <ul class="hidden md:flex space-x-6 items-center">
      <li>
        <NuxtLink to="/" class="flex items-center hover:text-blue-400 transition">
          <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24"><path fill="currentColor" d="M5 17V6zm5 5v-6.225L11.65 11h8.7L22 15.775V22h-2v-1.5h-8V22zm1.85-7h8.3l-.875-2.5h-6.55zm1.65 3.75q.425 0 .713-.288t.287-.712t-.288-.712t-.712-.288t-.712.288t-.288.712t.288.713t.712.287m5 0q.425 0 .713-.288t.287-.712t-.288-.712t-.712-.288t-.712.288t-.288.712t.288.713t.712.287m-7 .25h9v-2.5h-9zM1 20v-3h2V4h17v5h-2V6H5v11h3v3zm10.5-1v-2.5z"/></svg>
        </NuxtLink>
      </li>
      <li>
        <NuxtLink to="/guardados" class="flex items-center hover:text-blue-400 transition">
          <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24"><path fill="currentColor" fill-opacity="0" d="M12 3l2.35 5.76l6.21 0.46l-4.76 4.02l1.49 6.04l-5.29 -3.28l-5.29 3.28l1.49 -6.04l-4.76 -4.02l6.21 -0.46Z"><animate fill="freeze" attributeName="fill-opacity" begin="0.5s" dur="0.5s" values="0;1"/></path><path fill="none" stroke="currentColor" stroke-dasharray="36" stroke-dashoffset="36" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3L9.65 8.76L3.44 9.22L8.2 13.24L6.71 19.28L12 16M12 3L14.35 8.76L20.56 9.22L15.8 13.24L17.29 19.28L12 16"><animate fill="freeze" attributeName="stroke-dashoffset" dur="0.5s" values="36;0"/><animate attributeName="d" dur="1.5s" repeatCount="indefinite" values="M12 3L9.65 8.76L3.44 9.22L8.2 13.24L6.71 19.28L12 16M12 3L14.35 8.76L20.56 9.22L15.8 13.24L17.29 19.28L12 16;M12 7L10.82 10.38L7.24 10.45L10.1 12.62L9.06 16.05L12 14M12 7L13.18 10.38L16.76 10.45L13.9 12.62L14.94 16.05L12 14;M12 3L9.65 8.76L3.44 9.22L8.2 13.24L6.71 19.28L12 16M12 3L14.35 8.76L20.56 9.22L15.8 13.24L17.29 19.28L12 16"/></path></svg>
        </NuxtLink>
      </li>
      <li>
        <NuxtLink to="/vender" class="flex items-center hover:text-blue-400 transition">
          <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24"><path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M13.5 15.38v6a1.5 1.5 0 0 0 1.5 1.5h1.5m-12-7.5H3a1.5 1.5 0 0 0-1.5 1.5c0 2.25 3 2.25 3 4.5a1.5 1.5 0 0 1-1.5 1.5H1.5m6 0v-6a1.5 1.5 0 0 1 3 0v6m-3-3h3m12 3H21a1.5 1.5 0 0 1-1.5-1.5v-4.5a1.5 1.5 0 0 1 1.5-1.5h1.5m-3 4.5h3M.75 7.87a2.82 2.82 0 1 0 5.63 0a2.81 2.81 0 1 0 5.62 0a2.818 2.818 0 0 0 3.933 2.757A2.82 2.82 0 0 0 17.63 7.87a2.81 2.81 0 1 0 5.62 0l-1.37-6.16a.76.76 0 0 0-.73-.59H2.85a.76.76 0 0 0-.73.59z"/></svg>
        </NuxtLink>
      </li>
    </ul>

    <!-- Botones de usuario -->
    <div class="flex items-center space-x-5">
      <template v-if="user">
        <NuxtLink to="/perfil" class="flex items-center hover:text-blue-400 transition">
           <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-dasharray="28" stroke-dashoffset="28" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"><path d="M4 21v-1c0 -3.31 2.69 -6 6 -6h4c3.31 0 6 2.69 6 6v1"><animate fill="freeze" attributeName="stroke-dashoffset" dur="0.4s" values="28;0"/></path><path d="M12 11c-2.21 0 -4 -1.79 -4 -4c0 -2.21 1.79 -4 4 -4c2.21 0 4 1.79 4 4c0 2.21 -1.79 4 -4 4Z"><animate fill="freeze" attributeName="stroke-dashoffset" begin="0.4s" dur="0.4s" values="28;0"/></path></g></svg>
          <span class="ml-2 hidden md:block">{{ nombreUsuario }}</span>
        </NuxtLink>

        <NuxtLink to="modificar" class="flex items-center hover:text-blue-400 transition">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 15 15"><path fill="currentColor" d="M11 8c.55 0 1 .45 1 1v3h-1v1c0 .55-.45 1-1 1s-1-.45-1-1v-1H6v1c0 .55-.45 1-1 1s-1-.45-1-1v-1H3V9c0-.55.45-1 1-1l.89-2.66c.07-.2.26-.34.47-.34h4.28c.21 0 .4.14.47.34zm-6 3c.55 0 1-.45 1-1s-.45-1-1-1s-1 .45-1 1s.45 1 1 1m0-3h5l-.62-2H5.62zm5 3c.55 0 1-.45 1-1s-.45-1-1-1s-1 .45-1 1s.45 1 1 1M2 5.41V14H0V4.09L7.5.91L15 4.09V14h-2V5.41L7.5 3.09z"/></svg>
        </NuxtLink>

        <NuxtLink @click="logout" class="flex items-center hover:text-blue-400 transition">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"><path stroke-dasharray="36" stroke-dashoffset="36" d="M13 4l7 0c0.55 0 1 0.45 1 1v14c0 0.55 -0.45 1 -1 1h-7"><animate fill="freeze" attributeName="stroke-dashoffset" dur="0.5s" values="36;0"/></path><path stroke-dasharray="14" stroke-dashoffset="14" d="M3 12h11.5"><animate fill="freeze" attributeName="stroke-dashoffset" begin="0.6s" dur="0.2s" values="14;0"/></path><path stroke-dasharray="6" stroke-dashoffset="6" d="M14.5 12l-3.5 -3.5M14.5 12l-3.5 3.5"><animate fill="freeze" attributeName="stroke-dashoffset" begin="0.8s" dur="0.2s" values="6;0"/></path></g></svg>
        </NuxtLink>
        </template>

      <template v-else>
        <NuxtLink
          to="/login"
          class=" flex bg-blue-600 hover:bg-blue-700 text-white px-5 py-2 rounded-md text-sm transition"
        > <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="1 1 25 26"><path fill="currentColor" fill-rule="evenodd" d="M12 4a8 8 0 0 0-6.96 11.947A4.99 4.99 0 0 1 9 14h6a4.99 4.99 0 0 1 3.96 1.947A8 8 0 0 0 12 4m7.943 14.076q.188-.245.36-.502A9.96 9.96 0 0 0 22 12c0-5.523-4.477-10-10-10S2 6.477 2 12a9.96 9.96 0 0 0 2.057 6.076l-.005.018l.355.413A9.98 9.98 0 0 0 12 22q.324 0 .644-.02a9.95 9.95 0 0 0 5.031-1.745a10 10 0 0 0 1.918-1.728l.355-.413zM12 6a3 3 0 1 0 0 6a3 3 0 0 0 0-6" clip-rule="evenodd"/></svg>
          Iniciar sesión
        </NuxtLink>
        <NuxtLink
          to="/register"
          class="border border-blue-600 hover:bg-blue-600 hover:text-white px-4 py-2 rounded-md text-sm transition"
        >
          Registrarse
        </NuxtLink>
      </template>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
const router = useRouter()

const { $supabase } = useNuxtApp()
const user = ref(null)
const nombreUsuario = ref('')
let authListener = null


const getUserData = async () => {
  const { data: { user: authUser } } = await $supabase.auth.getUser()
  user.value = authUser

  if (user.value) {
    const { data, error } = await $supabase
      .from('usuarios')
      .select('nombre')
      .eq('id', user.value.id)
      .single()
    
    if (!error && data) {
      nombreUsuario.value = data.nombre
    }
  }
}

const logout = async () => {
  await $supabase.auth.signOut()
  console.log('saliste')
  router.push('/')
}
onMounted(async () => {
  await getUserData()

  // Escuchar cambios de sesión
  authListener = $supabase.auth.onAuthStateChange((_event, session) => {
    user.value = session?.user ?? null
    if (!user.value) nombreUsuario.value = ''
    else getUserData()
  })
})

onUnmounted(() => {
  if (authListener) authListener.subscription.unsubscribe()
})


</script>




<style scoped>
nav {
  position: sticky;
  top: 0;
  z-index: 50;
}
</style>

