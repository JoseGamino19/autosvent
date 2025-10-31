<script setup>
import { ref, onMounted } from 'vue'
const { $supabase } = useNuxtApp()

const usuario = ref(null)
const errorMsg = ref('')
const loading = ref(true)
const email = ref('')

onMounted(async () => {
  try {
    const { data: { user } } = await $supabase.auth.getUser()
    email.value=user.email

    if (!user) {
      errorMsg.value = 'No hay usuario logueado'
      loading.value = false
      return
    }

    const { data, error } = await $supabase
      .from('usuarios')
      .select('*')
      .eq('id', user.id)
      .single()

    if (error) throw error
    usuario.value = data
  } catch (error) {
    errorMsg.value = error.message
  } finally {
    loading.value = false
  }
})


</script>

<template>
  <div class="flex justify-center items-center min-h-screen bg-blue-100">
    <div
      v-if="loading"
      class="text-gray-600 text-lg font-semibold"
    >
      Cargando datos del usuario...
    </div>

    <div
      v-else-if="errorMsg"
      class="bg-red-100 text-red-600 p-4 rounded-lg shadow-md"
    >
      {{ errorMsg }}
    </div>

    <div
      v-else-if="usuario"
      class="bg-white rounded-2xl shadow-lg p-6 w-full max-w-md"
    >
      <div class="flex flex-col items-center">
        <img
          src="https://cdn-icons-png.flaticon.com/512/3135/3135715.png"
          alt="Usuario"
          class="w-24 h-24 rounded-full mb-4 border-4 border-blue-200"
        />
        <h2 class="text-2xl font-semibold text-gray-800">
          {{ usuario.nombre }}
        </h2>
        <p class="text-gray-500 mb-4">{{ usuario.ciudad }}</p>
      </div>

      <div class="border-t border-gray-200 mt-4 pt-4 space-y-2">
        <p><strong>Correo:</strong> {{ email || 'Sin correo asociado' }}</p>
        <p><strong>Edad:</strong> {{ usuario.edad || 'No especificada' }}</p>
        <p><strong>Teléfono:</strong> {{ usuario.telefono || 'No registrado' }}</p>
        <p><strong>Domicilio:</strong> {{ usuario.domicilio || 'Sin domicilio' }}</p>
      </div>
    </div>
  </div>
</template>
