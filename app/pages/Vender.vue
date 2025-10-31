<script setup>
import { ref, onMounted } from 'vue'
const { $supabase } = useNuxtApp()
const router = useRouter()

const user = ref(null)
const marca = ref('')
const linea = ref('')
const modelo = ref('')
const tipo = ref('')
const precio = ref('')
const tipo_manejo = ref('')
const cilindraje = ref('')
const descripcion = ref('')
const imagen = ref(null)
const isLoading = ref(false)
const errorMsg = ref('')
const successMsg = ref('')
const id = ref('')

// 🔹 Obtener usuario actual
const getUser = async () => {
  const { data } = await $supabase.auth.getUser()
  user.value = data?.user || null

  id.value=user.value.id
}
onMounted(getUser)

// 🔹 Manejar archivo
const handleFileChange = (event) => {
  imagen.value = event.target.files[0]
}

// 🔹 Subir imagen al bucket y obtener URL
const uploadImage = async (file) => {
  const fileName = `${user.value.id}-${Date.now()}-${file.name}`
  const filePath = `imagenes/${fileName}`

  const { error } = await $supabase.storage
    .from('autos')
    .upload(filePath, file)

  if (error) throw error

  const { data } = $supabase.storage
    .from('autos')
    .getPublicUrl(filePath) 

  return data.publicUrl
}


const handleRegister = async () => {
  try {
    isLoading.value = true
    errorMsg.value = ''
    successMsg.value = ''

    if (!user.value) throw new Error('Debes iniciar sesión para publicar un auto')

    let imageUrl = null
    if (imagen.value) {
      imageUrl = await uploadImage(imagen.value)
    } else {
      throw new Error('Por favor selecciona una imagen')
    }

    const { error } = await $supabase
      .from('autos')
      .insert([
        {
          marca: marca.value,
          linea: linea.value,
          modelo: modelo.value,
          tipo: tipo.value,
          tipo_manejo: tipo_manejo.value,
          precio: precio.value,
          cilindraje: cilindraje.value,
          descripcion: descripcion.value,
          imagen: imageUrl,
          id_usuario: id.value
        },
      ])


    if (error) throw error

    successMsg.value = 'Auto publicado con éxito 🚗'
    router.push('/')
    // Limpia formulario
    marca.value = linea.value = modelo.value = tipo.value = precio.value = descripcion.value = cilindraje.value = tipo_manejo.value = ''
    imagen.value = null 
  } catch (err) {
    errorMsg.value = err.message
  } finally {
    isLoading.value = false
  }

}
</script>

<template>
  <div class="bg-blue-100 min-h-screen" v-if="!user">
    <h1 class="text-center p-6 text-3xl uppercase font-bold text-gray-900">
      Aquí podrás publicar tus autos para venderlos
    </h1>
    <h2 class="text-center text-2xl text-gray-900">
      Inicia sesión para poder iniciar con la venta...
    </h2>
  </div>

  <div v-else class="bg-blue-100 min-h-screen flex flex-col items-center justify-center p-5">
    <div class="bg-white shadow-lg rounded-lg p-8 w-115">
      <form @submit.prevent="handleRegister">
        <div class="flex justify-center items-center mb-4">
          <svg xmlns="http://www.w3.org/2000/svg" width="35" height="35"  class="text-blue-600" viewBox="0 0 24 24"><g fill="none" stroke="currentColor" stroke-linecap="round" stroke-width="1.5"><path d="M6.986 3.7c2.797 3.095 7.41-3.584 10.14-1.16c1.57 1.394 1.073 4.474-.965 6.48"/><path stroke-linejoin="round" d="M13.79 13.984c.018-.335.111-.947-.397-1.412m0 0a1.9 1.9 0 0 0-.666-.377c-1.048-.37-2.336.867-1.425 2c.49.608.867.795.832 1.486c-.025.486-.503.994-1.132 1.188c-.547.168-1.15-.055-1.531-.481c-.466-.52-.42-1.011-.423-1.225m4.345-2.59l.574-.575m-4.455 4.455l-.545.545"/><path d="M18.273 6.633c.925.178 1.133.762 1.409 2.384c.249 1.46.319 3.213.319 3.96a1.3 1.3 0 0 1-.319.74c-1.935 2.028-5.776 5.858-7.714 7.76c-.76.68-1.908.695-2.716.071c-1.653-1.487-3.241-3.168-4.797-4.685c-.625-.806-.61-1.95.07-2.709c2.051-2.127 5.762-5.768 7.856-7.78c.21-.18.468-.292.743-.317c.47 0 1.276.063 2.062.108"/></g></svg>
          <h1 class="text-2xl text-blue-600 font-bold">Vender auto</h1>
        </div>

        <input v-model="marca" type="text" placeholder="Marca del auto" required class="w-full mb-2 p-2 border rounded"/>
        <input v-model="linea" type="text" placeholder="Línea del auto" required class="w-full mb-2 p-2 border rounded"/>
        <input v-model="modelo" type="text" placeholder="Modelo del auto" required class="w-full mb-2 p-2 border rounded"/>
        <input v-model="tipo_manejo" type="text" placeholder="Manejo del auto" required class="w-full mb-2 p-2 border rounded"/>
        <input v-model="cilindraje" type="text" placeholder="Cilindros del auto" required class="w-full mb-2 p-2 border rounded"/>
        <input v-model="tipo" type="text" placeholder="Tipo de auto" required class="w-full mb-2 p-2 border rounded"/>
        <input v-model="precio" type="text" placeholder="Precio del auto" required class="w-full mb-2 p-2 border rounded"/>
        <input v-model="descripcion" type="text" placeholder="Descripción del auto" required class="w-full mb-2 p-2 border rounded"/>

        <input type="file" @change="handleFileChange" accept="image/*" required class="w-full mb-4"/>
        <button type="submit" :disabled="isLoading" class="bg-blue-600 hover:bg-blue-700 text-white w-full py-2 rounded-md">
          {{ isLoading ? 'Publicando...' : 'Publicar auto' }}
        </button>
      </form>

      <!-- Mensajes -->
      <p v-if="errorMsg" class="text-red-500 text-center mt-3">{{ errorMsg }}</p>
      <p v-if="successMsg" class="text-green-600 text-center mt-3">{{ successMsg }}</p>
    </div>
  </div>
</template>

<style scoped>

input {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
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
