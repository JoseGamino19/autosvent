<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
const { $supabase } = useNuxtApp()


interface AutoGuardado {
  id_favorito: string
  marca: string
  linea: string
  modelo: string
  tipo: string
  precio : string
  id_usuario: string
  id_auto: string
  imagen: string;
}

const user = ref(null)
const autoGuardado = ref<AutoGuardado[]>([])
let authListener: any = null 


const getUserData = async () => {
  const { data, error } = await $supabase.auth.getUser()
  if (error || !data.user) return

  user.value = data.user

  const { data: favoritos, error: favError } = await $supabase
    .from('favoritos')
    .select('*')
    .eq('id_usuario', user.value.id)

  if (!favError && favoritos) {
    autoGuardado.value = favoritos
  }
}


onMounted(async () => {
  await getUserData()

  const { data: listener } = $supabase.auth.onAuthStateChange((_event, session) => {
    user.value = session?.user ?? null
    if (user.value) {
      getUserData() 
    } else {
      autoGuardado.value = [] 
    }
  })
  authListener = listener
})

onUnmounted(() => {
  if (authListener) authListener.subscription.unsubscribe()
})

const borrarFavorito = async (id_favorito: any) => {
  const { error } = await $supabase
  .from('favoritos')
  .delete()
  .eq('id_favorito',id_favorito)

  getUserData()

  
}

</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 py-8 px-4 bg-blue-100">
    <div class="max-w-7xl mx-auto">
      <!-- Header -->
      <div class="mb-8">
        <h1 class="text-4xl font-bold text-gray-900 mb-2">Mis Autos Favoritos</h1>
        <p class="text-gray-600">{{ autoGuardado.length }} {{ autoGuardado.length === 1 ? 'vehículo guardado' : 'vehículos guardados' }}</p>
      </div>

      <!-- Empty State -->
      <div v-if="autoGuardado.length === 0" class="text-center py-16 bg-blue-100">
        <svg class="mx-auto h-24 w-24 text-gray-400 mb-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="currentColor" fill-opacity="0" d="M12 3l2.35 5.76l6.21 0.46l-4.76 4.02l1.49 6.04l-5.29 -3.28l-5.29 3.28l1.49 -6.04l-4.76 -4.02l6.21 -0.46Z"><animate fill="freeze" attributeName="fill-opacity" begin="0.5s" dur="0.5s" values="0;1"/></path><path fill="none" stroke="currentColor" stroke-dasharray="36" stroke-dashoffset="36" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3l-2.35 5.76l-6.21 0.46l4.76 4.02l-1.49 6.04l5.29 -3.28M12 3l2.35 5.76l6.21 0.46l-4.76 4.02l1.49 6.04l-5.29 -3.28"><animate fill="freeze" attributeName="stroke-dashoffset" dur="0.5s" values="36;0"/></path></svg>
        </svg>
        <h3 class="text-xl font-semibold text-gray-900 mb-2">No tienes favoritos aún</h3>
        <p class="text-gray-600 mb-6">inicia seccion o explora nuestro catálogo y guarda tus autos favoritos</p>
      </div>

  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 p-6 bg-blue-100">
    <div
      v-for="auto in autoGuardado"
      :key="auto.id_auto"
      class="bg-white rounded-xl shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300">
      <img
        v-if="auto.imagen"
        :src="auto.imagen"
        alt="auto"
        class="object-cover hover:scale-105 transition-transform duration-300"
      />
      <div class="p-5">
        <div class="flex justify-between items-start mb-3">
          <div>
            <h3 class="text-lg font-bold text-gray-900">{{ auto.linea }}</h3>
            <p class="text-sm text-gray-600">{{ auto.marca }} • {{ auto.modelo }}</p>
          </div>
          <span class="bg-blue-100 text-blue-800 text-xs font-semibold px-2.5 py-1 rounded">
          {{ auto.tipo }}
          </span>
        </div>
      </div>
      <div class="flex justify-between items-center pt-2 border-t border-gray-200">
        <div>
          <p class="text-2xl font-bold text-gray-900 ml-6">${{ auto.precio}}</p>
        </div>
        <button class="bg-blue-600 text-white px-4 py-2 m-2 rounded-lg hover:bg-blue-700 transition text-sm font-semibold" @click="borrarFavorito(auto.id_favorito)"> 
          eliminar
        </button>
      </div>
  </div>
</div>
    </div>
  </div>
</template>