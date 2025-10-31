<template>
  <div class="p-15 flex flex-col justify-center items-center bg-blue-100">
    <div class="bg-white shadow-lg rounded-lg p-8 w-115">
      <form @submit.prevent="handleRegister" class="">
        <div class="flex justify-center items-center">
          <svg class="text-blue-600" xmlns="http://www.w3.org/2000/svg" width="60" height="60" viewBox="0 0 24 24"><path fill="currentColor" fill-rule="evenodd" d="M4 4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V6a2 2 0 0 0-2-2zm10 5a1 1 0 0 1 1-1h3a1 1 0 1 1 0 2h-3a1 1 0 0 1-1-1m0 3a1 1 0 0 1 1-1h3a1 1 0 1 1 0 2h-3a1 1 0 0 1-1-1m0 3a1 1 0 0 1 1-1h3a1 1 0 1 1 0 2h-3a1 1 0 0 1-1-1m-8-5a3 3 0 1 1 6 0a3 3 0 0 1-6 0m1.942 4a3 3 0 0 0-2.847 2.051l-.044.133l-.004.012c-.042.126-.055.167-.042.195c.006.013.02.023.038.039c.032.025.08.064.146.155A1 1 0 0 0 6 17h6a1 1 0 0 0 .811-.415a.7.7 0 0 1 .146-.155c.019-.016.031-.026.038-.04c.014-.027 0-.068-.042-.194l-.004-.012l-.044-.133A3 3 0 0 0 10.059 14z" clip-rule="evenodd"/></svg>
          <h1 class="text-2xl text-blue-600 font-bold">Registro</h1>
        </div>
      <div class="w-100 text-black">
        <input 
         v-model="nombre" 
          type="text" 
          placeholder="Nombre de usuario"
          required
        />
      </div>  
      <div class="w-100">
        <input 
         v-model="email" 
          type="email" 
          placeholder="Email"
          required
        />
      </div>
      
      <div class="w-100">
        <input 
          v-model="password" 
          type="password" 
          placeholder="Contraseña"
          required
        />
      </div>

      <div class="w-100">
        <input 
          v-model="edad" 
          type="text" 
          placeholder="Edad"
          required
        />
      </div>

      <div class="w-100">
        <input 
          v-model="telefono" 
          type="text" 
          placeholder="Telefono"
          required
        />
      </div>

      <div class="w-100">
        <input 
          v-model="domicilio" 
          type="text" 
          placeholder="Domicilio"
          required
        />
      </div>

      <div class="w-100">
        <input 
          v-model="ciudad" 
          type="text" 
          placeholder="Ciudad"
          required
        />
      </div>
    
      
      <button type="submit" :disabled="isLoading" class=" text-center bg-blue-600 hover:bg-blue-700 text-white w-100 py-2 rounded-md">
        {{ isLoading ? 'Cargando...' : 'Crear cuenta' }}
      </button>
    </form>

    <p v-if="error" class="error">{{ error }}</p>
  </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const { $supabase } = useNuxtApp()
const router = useRouter()

const email = ref('')
const password = ref('')
const nombre = ref('')
const telefono = ref('')
const edad = ref('')
const domicilio = ref('')
const ciudad = ref('')
const isLoading = ref(false)

const errorMsg = ref('')
const successMsg = ref('')

const handleRegister = async () => {
  isLoading.value = true
  errorMsg.value = ''
  successMsg.value = ''

  const { data, error } = await $supabase.auth.signUp({
    email: email.value,
    password: password.value,
  })

  if (error) {
    errorMsg.value = error.message
    isLoading.value = false
    return
  }

  const { user } = data

  const { error: insertError } = await $supabase.from('usuarios').insert({
    id: user.id,
    nombre: nombre.value,
    telefono: telefono.value,
    edad: edad.value,
    domicilio: domicilio.value,
    ciudad: ciudad.value
  })

  if (insertError) {
    errorMsg.value = insertError.message
  } else {
    successMsg.value = 'Usuario registrado correctamente. Revisa tu correo para confirmar.'
    router.push('/login')
  }

  isLoading.value = false
}
</script>


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


