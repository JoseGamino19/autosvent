<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const { $supabase } = useNuxtApp()
const router = useRouter()

const autos = ref([])
const loading = ref(true)
const errorMsg = ref('')
const successMsg = ref('')
const editando = ref(null) // auto actualmente en edición

// Campos de edición
const editForm = ref({
  marca: '',
  tipo:'',
  cilindraje:'',
  linea:'',
  modelo: '',
  tipo_manejo: '',
  descripcion:'',
  precio: ''
})

//  Cargar autos creados por el usuario 
onMounted(async () => {
  const { data: { user } } = await $supabase.auth.getUser()
  if (!user) {
    router.push('/login')
    return
  }

  const { data, error } = await $supabase
    .from('autos')
    .select('*')
    .eq('id_usuario', user.id)

  if (error) {
    errorMsg.value = error.message
  } else {
    autos.value = data
  }
  loading.value = false
})

//  Eliminar un auto
const eliminarAuto = async (id) => {
  const confirmar = confirm('¿Seguro que quieres eliminar este auto?')
  if (!confirmar) return

  const { error } = await $supabase
  .from('autos')
  .delete()
  .eq('id_auto', id)

  if (error) {
    errorMsg.value = error.message
  } else {
    autos.value = autos.value.filter(a => a.id_auto !== id)
    successMsg.value = 'Auto eliminado correctamente ✅'
  }
}

// Editar un auto
const empezarEdicion = (auto) => {
  editando.value = auto.id_auto
  editForm.value = { ...auto }
}

// Guardar cambios
const guardarCambios = async () => {
  const { error } = await $supabase
    .from('autos')
    .update({
      marca: editForm.value.marca,
      modelo: editForm.value.modelo,
      precio: editForm.value.precio,
      tipo: editForm.value.tipo,
      cilindraje: editForm.value.cilindraje,
      linea: editForm.value.linea,
      tipo_manejo: editForm.value.precio,
      descripcion:editForm.value.descripcion
    })
    .eq('id_auto', editando.value)

  if (error) {
    errorMsg.value = error.message
  } else {
    const index = autos.value.findIndex(a => a.id_auto === editando.value)
    autos.value[index] = { ...autos.value[index], ...editForm.value }
    successMsg.value = 'Auto actualizado correctamente ✅'
    editando.value = null
  }
}
</script>

<template>
  <div class="p-6">
    <h1 class="text-3xl font-bold mb-6 text-center text-gray-800">Mis Autos</h1>

    <div v-if="loading" class="text-center text-gray-500">Cargando autos...</div>
    <div v-if="errorMsg" class="text-red-600 text-center">{{ errorMsg }}</div>
    <div v-if="successMsg" class="text-green-600 text-center">{{ successMsg }}</div>

    <div v-if="!loading && autos.length === 0" class="text-center text-gray-500 mt-4">
      No tienes autos registrados aún.
    </div>

    <div class="grid gap-4 md:grid-cols-2 lg:grid-cols-3 mt-6">
      <div
        v-for="auto in autos"
        :key="auto.id"
        class="border p-4 rounded-lg shadow-sm bg-white"
      >
      
        <div v-if="editando === auto.id_auto">
          <input v-model="editForm.marca" placeholder="Marca" class="border p-2 w-full mb-2 rounded" />
          <input v-model="editForm.linea" placeholder="Linea" class="border p-2 w-full mb-2 rounded" />
          <input v-model="editForm.modelo" placeholder="Modelo" class="border p-2 w-full mb-2 rounded" />
          <input v-model="editForm.cilindraje" placeholder="Cilindros" class="border p-2 w-full mb-2 rounded" />
          <input v-model="editForm.tipo" placeholder="tipo" class="border p-2 w-full mb-2 rounded" />
          <input v-model="editForm.tipo_manejo" placeholder="Tipo de manejo" class="border p-2 w-full mb-2 rounded" />
          <input v-model="editForm.descripcion" placeholder="Descripcion" class="border p-2 w-full mb-2 rounded" />
          <input v-model="editForm.precio" placeholder="Precio" class="border p-2 w-full mb-2 rounded" />

          <div class="flex gap-2">
            <button @click="guardarCambios" class="bg-green-600 text-white px-3 py-1 rounded hover:bg-green-700">
              Guardar
            </button>
            <button @click="editando = null" class="bg-gray-400 text-white px-3 py-1 rounded hover:bg-gray-500">
              Cancelar
            </button>
          </div>
        </div>

        <div v-else>
          <h2 class="text-xl font-bold">{{ auto.marca }} {{ auto.linea }}</h2>
          <p class="text-gray-600">Año: {{ auto.modelo }}</p>
          <p class="text-gray-600">Precio: ${{ auto.precio }}</p>

          <div class="flex gap-2 mt-3">
            <button
              @click="empezarEdicion(auto)"
              class="bg-blue-600 text-white px-3 py-1 rounded hover:bg-blue-700"
            >
              Editar
            </button>
            <button
              @click="eliminarAuto(auto.id_auto)"
              class="bg-red-600 text-white px-3 py-1 rounded hover:bg-red-700"
            >
              Borrar
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
