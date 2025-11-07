<script setup lang="ts">
import { ref, onMounted } from 'vue';
const { $supabase } = useNuxtApp();
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()
const listauto = ref<AutoDetail[]>([]);
const errorMsg = ref('');
const expandedAuto = ref<AutoDetail | null>(null)
const user = ref(null)
const Msg = ref('')

interface AutoDetail {
  id_auto: string;
  marca: string;
  tipo: string;
  cilindraje: number;
  modelo: number;
  linea: string;
  tipo_manejo: string;
  descripcion: string;
  imagen: string;
  precio: number;
}


// Recuperar la sesión actual
onMounted(async () => {
  const { data, error } = await $supabase.auth.getUser()

  if (error) {
    console.error('Error al obtener el usuario:', error.message)
  } else {
    user.value = data.user
  }
})



const guardarAuto = async () => {
  if (!expandedAuto.value || !user.value) return confirm('⚠️ Inicia seccion para guardar favoritos.')

  const { data: existente, error: errorCheck } = await $supabase
    .from('favoritos')
    .select('id_auto')
    .eq('id_auto', expandedAuto.value.id_auto)
    .eq('id_usuario', user.value.id)
    .single()

    if (existente) {
      const confirmar = confirm('⚠️ Este auto ya está en tus favoritos.')
    return
  }
  else{
    Msg.value = 'Agregado en favoritos'
  }

  const { data, error } = await $supabase
    .from('favoritos')
    .insert([
      {
        id_auto: expandedAuto.value.id_auto,
        id_usuario: user.value.id,
        marca: expandedAuto.value.marca,
        linea: expandedAuto.value.linea,
        modelo: expandedAuto.value.modelo,
        tipo: expandedAuto.value.tipo,
        precio: expandedAuto.value.precio,
        imagen: expandedAuto.value.imagen,
      },
    ])
    .select()

}

const openCard = (auto: AutoDetail) => {
  expandedAuto.value = auto
}

const closeCard = () => {
  expandedAuto.value = null
  Msg.value=''
}

onMounted(async () => {
  try {
    const { data, error } = await $supabase.from('autos').select('*');
    if (error) throw error;
    listauto.value = data ?? [];
  } catch (err: any) {
    errorMsg.value = err.message;
  }
});

const filters = ref({
  search: '',        // Texto de búsqueda
})

const readQueryParams = () => {
  filters.value.search = Array.isArray(route.query.search)
    ? route.query.search[0] || ''
    : route.query.search || ''
}

const updateQueryParams = () => {
  const query = {}

  if (filters.value.search) query.search = filters.value.search

  router.push({ query })
}

watch(() => route.query, readQueryParams, { immediate: true })
const isAdmin = computed(() => user.value?.id === '8ac67f4f-6fbc-483a-8c8e-f0ed0bb0df7a')

const filteredAutos = computed(() => {
  let result = listauto.value

  if (filters.value.search) {
    const searchLower = filters.value.search.toLowerCase()
    result = result.filter(post =>
      post.linea.toLowerCase().includes(searchLower) ||
      post.marca.toLowerCase().includes(searchLower) ||
      post.tipo.toLowerCase().includes(searchLower) ||
      post.modelo.toString().includes(searchLower)
    )
  }

  return result
})

const eliminarAuto = async (id_auto: string) => {
  if (user.value.id !== '8ac67f4f-6fbc-483a-8c8e-f0ed0bb0df7a') {
    return alert('⚠️ No tienes permisos para eliminar autos.')
  }

  const confirmar = confirm('¿Estás seguro de que quieres eliminar este auto?')
  if (!confirmar) return

  try {
    const { error } = await $supabase
      .from('autos')
      .delete()
      .eq('id_auto', id_auto)

    if (error) throw error

    listauto.value = listauto.value.filter(a => a.id_auto !== id_auto)

    alert('✅ Auto eliminado correctamente.')
  } catch (err: any) {
    console.error('Error al eliminar:', err.message)
    alert('❌ Error al eliminar el auto.')
  }
}

</script>

<template>
  <div class="bg-blue-100">
    <h1 class="text-center p-6 text-3xl uppercase font-bold text-gray-900">AutosVent</h1>
    <h2 class="text-center text-2xl text-gray-900">El mejor lugar para comprar, verificar y conducir...</h2>
  </div >
  <div class=" flex justify-center bg-blue-100 ">
    <input
    
      v-model="filters.search"
      @input="updateQueryParams"
      type="text"
      placeholder="Buscar por modelo, marca o linea "
    />
  </div>

  <!-- Tarjetas -->
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 p-6 bg-blue-100 ">
    <div
      v-for="auto in filteredAutos"
      :key="auto.id_auto"
      class="bg-white rounded-xl shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300"
      @click="openCard(auto)">
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
            <p class="text-lg text-gray-600">{{ auto.marca }} • {{ auto.modelo }}</p>
          </div>
          <span class="bg-blue-100 text-blue-800 text-sm font-semibold px-2.5 py-1 rounded">
          {{ auto.tipo }}
          </span>
        </div>
      </div>
      <div class="flex items-center gap-4 mb-4 text-lg text-gray-600 ml-4">
        <div class="flex items-center gap-1">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
          </svg>
          <span>{{ auto.tipo_manejo }}</span>
        </div>
        <div class="flex items-center gap-1" >
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 72 72">
            <path fill="#9b9b9a" d="M16.564 17.064h37.685l3.125 10.847h2.758v14.086h-3.171l-6.572 14.982h-31.16L13.255 42.77h-2.206V26.532h2.206z"/><circle cx="25.388" cy="29.275" r="5.883" fill="#d0cfce"/><circle cx="27.41" cy="46.11" r="4.044" fill="#d0cfce"/><circle cx="44.322" cy="36.023" r="7.85" fill="#d0cfce"/><path fill="#fff" d="M20.75 15.944c0-1.595 1.175-2.888 2.625-2.888S26 14.35 26 15.944m2.625 0c0-1.595 1.175-2.888 2.625-2.888s2.625 1.293 2.625 2.888m2.625 0c0-1.595 1.175-2.888 2.625-2.888s2.625 1.293 2.625 2.888m2.625 0c0-1.595 1.175-2.888 2.625-2.888s2.625 1.293 2.625 2.888"/><g fill="none" stroke="#000" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="10" stroke-width="2"><path d="M16.564 17.064h37.685l3.125 10.847h2.758v14.086h-3.171l-6.572 14.982h-31.16L13.255 42.77h-2.206V26.532h2.206z"/><circle cx="25.388" cy="29.275" r="5.883"/><circle cx="27.41" cy="46.11" r="4.044"/><circle cx="44.322" cy="36.023" r="7.85"/><path d="m25.388 23.392l18.934 4.781m-24.817 1.102s2.252 14.997 3.86 18.444s4.045 2.435 4.045 2.435c4.044-.367 20.91-7.384 20.91-7.384M20.75 15.944c0-1.595 1.175-2.888 2.625-2.888S26 14.35 26 15.944m2.625 0c0-1.595 1.175-2.888 2.625-2.888s2.625 1.293 2.625 2.888m2.625 0c0-1.595 1.175-2.888 2.625-2.888s2.625 1.293 2.625 2.888m2.625 0c0-1.595 1.175-2.888 2.625-2.888s2.625 1.293 2.625 2.888"/></g>
          </svg>
          <span>{{ auto.cilindraje }} Cilindros</span>
      </div>
    </div>
    <div class="flex justify-between items-center pt-4 border-t border-gray-200">
      <div>
        <p class="text-2xl font-bold text-gray-900 m-3">${{ auto.precio }},000</p>
        <p class="text-ms text-gray-500 m-3">Precio de venta</p>
      </div>
          <div v-if="user && isAdmin" class="">
            <button
              @click="eliminarAuto(auto.id_auto)"
              class="bg-red-500 text-white py-3 px-4 m-3 rounded-md hover:bg-red-600 flex">
              Eliminar
            </button>
          </div>
    </div>
  </div>
</div>
  <!-- Modal expandido -->
  <transition name="fade">
    <div
      v-if="expandedAuto"
      class="fixed inset-0 bg-black/60 bg-opacity-50 flex items-center justify-center z-50 " 
      @click.self="closeCard"
    >
      <div
        class="bg-white/100 backdrop-blur-md rounded-xl shadow-2xl w-11/12 md:w-3/4 lg:w-2/3 xl:w-1/2 max-h-[90vh] overflow-y-auto p-6 relative"
      >
        <img
          v-if="expandedAuto.imagen"
          :src="expandedAuto.imagen"
          alt="auto"
          class="w-full h-90 object-cover rounded-md mb-4"
        />
        <h2 class="text-2xl font-bold mb-2">
          {{ expandedAuto.marca }} {{ expandedAuto.modelo }} {{ expandedAuto.linea }}
        </h2>
        <p class="text-gray-700 mb-2"><strong class="text-black">Tipo:</strong> {{ expandedAuto.tipo }}</p>
        <p class="text-gray-700 mb-2"><strong class="text-black">Cilindros:</strong> {{ expandedAuto.cilindraje }}</p>
        <p class="text-gray-700 mb-2"><strong class="text-black">Tipo de manejo:</strong> {{ expandedAuto.tipo_manejo }}</p>
        <p class="text-gray-700 mb-2"><strong class="text-black">Precio:</strong> {{ expandedAuto.precio }},000  </p>
        <p class="text-gray-700 mb-4"><strong class="text-black">Descripción:</strong> {{ expandedAuto.descripcion }}</p>
        <p v-if="Msg" class="text-green-600 text-center m-2">{{ Msg }}</p>
        

        <div class="flex space-x-4">
          <button
            @click="closeCard"
            class="bg-red-500 text-white px-4 py-3 rounded-md hover:bg-red-600 flex">
            Cerrar
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"><path stroke-dasharray="36" stroke-dashoffset="36" d="M13 4l7 0c0.55 0 1 0.45 1 1v14c0 0.55 -0.45 1 -1 1h-7"><animate fill="freeze" attributeName="stroke-dashoffset" dur="0.5s" values="36;0"/></path><path stroke-dasharray="14" stroke-dashoffset="14" d="M3 12h11.5"><animate fill="freeze" attributeName="stroke-dashoffset" begin="0.6s" dur="0.2s" values="14;0"/></path><path stroke-dasharray="6" stroke-dashoffset="6" d="M14.5 12l-3.5 -3.5M14.5 12l-3.5 3.5"><animate fill="freeze" attributeName="stroke-dashoffset" begin="0.8s" dur="0.2s" values="6;0"/></path></g></svg>
          </button>
          <button class="bg-yellow-500 p-3 text-white rounded-md hover:bg-yellow-600 flex" @click="guardarAuto">
            Guardar
            <svg xmlns="http://www.w3.org/2000/svg" class="" width="24" height="24" viewBox="0 0 512 512"><path fill="currentColor" d="M280 24c0-13.3-10.7-24-24-24s-24 10.7-24 24v80c0 13.3 10.7 24 24 24s24-10.7 24-24zm-94.2 200h140.3c6.8 0 12.8 4.3 15.1 10.6l19.1 53.4H151.7l19.1-53.4c2.3-6.4 8.3-10.6 15.1-10.6zm-75.3-10.9l-28.3 79.3C62.1 300.9 48 320.8 48 344v136c0 17.7 14.3 32 32 32h16c17.7 0 32-14.3 32-32v-32h256v32c0 17.7 14.3 32 32 32h16c17.7 0 32-14.3 32-32V344c0-23.2-14.1-43.1-34.2-51.6l-28.3-79.3C390.1 181.3 360 160 326.2 160H185.9c-33.8 0-64 21.3-75.3 53.1zM128 344a24 24 0 1 1 0 48a24 24 0 1 1 0-48m232 24a24 24 0 1 1 48 0a24 24 0 1 1-48 0M39 39c-9.4 9.4-9.4 24.6 0 33.9l48 48c9.4 9.4 24.6 9.4 33.9 0s9.4-24.6 0-33.9L73 39c-9.4-9.4-24.6-9.4-33.9 0zm400 0l-48 48c-9.4 9.4-9.4 24.6 0 33.9s24.6 9.4 33.9 0l48-48c9.4-9.4 9.4-24.6 0-33.9s-24.6-9.4-33.9 0"/></svg>
          </button>
        </div>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

input {
  background-color: white;
  width: 50%;
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #ffffff;
  border-radius: 9px;
}
</style>