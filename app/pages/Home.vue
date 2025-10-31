<script setup>
import { ref } from 'vue';

const tiposDeAutos = ref([
  { id: 1, 
    tipo: 'Sedán', 
    descripcion: 'Automóvil de cuatro puertas diseñado principalmente para uso familiar o personal, con buen confort y eficiencia.',
    usos_comunes: 'Transporte diario, Viajes urbanos, Familiares pequeños',
    imagen: "https://yzfixcquqfpklvciodcn.supabase.co/storage/v1/object/public/autos/detalles/unnamed.jpg"
  },
  { id: 2, 
    tipo: 'Hatchback', 
    descripcion: 'Vehículo compacto con una puerta trasera que se abre hacia arriba y un diseño más corto que el sedán.',
    usos_comunes: 'Uso urbano, Estudiantes, Parejas jóvenes',
    imagen: "https://yzfixcquqfpklvciodcn.supabase.co/storage/v1/object/public/autos/detalles/unnamed%20(1).jpg"
  },
  { id: 3, 
    tipo: 'Camioneta', 
    descripcion: 'Vehículo espacioso diseñado para familias grandes o transporte de personas.',
    usos_comunes: 'Familias numerosas, Transporte escolar, Servicios de traslado',
    imagen: "https://yzfixcquqfpklvciodcn.supabase.co/storage/v1/object/public/autos/detalles/unnamed%20(2).jpg"
  },
  { id: 4, 
    tipo: 'Pickup', 
    descripcion: 'Camioneta con una caja trasera abierta para carga, ideal para trabajo y transporte de materiales',
    usos_comunes: 'Trabajo en campo, Transporte de mercancía, Off-road',
    imagen: "https://yzfixcquqfpklvciodcn.supabase.co/storage/v1/object/public/autos/detalles/unnamed%20(3).jpg"
  },
  { id: 5, 
    tipo: 'Coupé', 
    descripcion: 'Auto deportivo de dos puertas, diseño aerodinámico y mayor rendimiento en carretera.',
    usos_comunes: 'Conducción deportiva, Uso personal, Carros de lujo',
    imagen: "https://yzfixcquqfpklvciodcn.supabase.co/storage/v1/object/public/autos/detalles/unnamed%20(4).jpg"
  },
  { id: 6, 
    tipo: 'Deportivo', 
    descripcion: 'Automóvil de alto rendimiento enfocado en velocidad, aceleración y manejo.',
    usos_comunes: 'ompetencias, Uso recreativo, Coleccionismo',
    imagen: "https://yzfixcquqfpklvciodcn.supabase.co/storage/v1/object/public/autos/detalles/unnamed%20(5).jpg"
  },
])


</script>


<template>
  <div class="min-h-screen flex flex-col items-center p-10 bg-blue-100">
    <h1 class="text-3xl font-bold text-blue-800 mb-4">Bienvenido a AutosVent</h1>
    <h2>Un sitio web donde podrás encontrar autos para todo tipo de gustos y estilos.</h2>
    <h2>Podrás ver una variedad de precios accesibles para ti.</h2>
    <h2>Y con una nueva funcionalidad este año, podrás publicar tu auto</h2>
    <h2>de manera gratuita y venderlo a miles de personas que visitan</h2>
    <h2>nuestra página. ¡Ahora puedes comenzar!</h2>

    <h1 class="text-3xl font-bold text-blue-800 m-4">¿Qué clase de auto necesitas? Puedo ayudarte</h1>

    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 p-6 bg-blue-100">
      <div
        v-for="tipo in tiposDeAutos"
        :key="tipo.id"
        class="bg-white shadow-md rounded-xl p-4 cursor-pointer transition hover:shadow-lg"
      >
        <img
          v-if="tipo.imagen"
          :src="tipo.imagen"
          alt="auto"
          class="w-full h-64 object-cover rounded-md mb-3"
        />
        <h3 class="text-lg font-semibold text-blue-700">{{ tipo.tipo }}</h3>
        <p class="font-bold mt-1">Descripción: {{ tipo.descripcion }}</p>
        <p class="font-bold mt-1">Usos comunes: {{ tipo.usos_comunes }}</p>
      </div>
    </div>
  </div>
</template>


