<script setup>
import { ref, computed } from 'vue'

const nombreFiltro = ref('')
const dniFiltro = ref('')

const personas = ref([
  { nombre: 'Juan', apellido: 'Pérez', dni: '12345678' },
  { nombre: 'María', apellido: 'García', dni: '87654321' },
  { nombre: 'Luis', apellido: 'López', dni: '45678912' }
])

const mostrarAdvertencia = computed(() => {
  const nombre = nombreFiltro.value.trim()
  const dni = dniFiltro.value.trim()

  return (
    (nombre && nombre.length < 3) ||
    (dni && dni.length < 3)
  )
})

const resultados = computed(() => {
  const nombre = nombreFiltro.value.trim().toLowerCase()
  const dni = dniFiltro.value.trim()

  if (!nombre && !dni) return []

  return personas.value.filter(p => {
    const nombreCompleto = `${p.nombre} ${p.apellido}`.toLowerCase()

    if (nombre && dni) {
      return nombreCompleto.includes(nombre) && p.dni.includes(dni)
    } else if (nombre) {
      return nombreCompleto.includes(nombre)
    } else if (dni) {
      return p.dni.includes(dni)
    }
  })
})
</script>

<template>
  <div class="container mt-4">
    <h2>Buscar persona</h2>
    <input
      v-model="nombreFiltro"
      type="text"
      placeholder="Nombre o Apellido"
      class="form-control my-2"
    />
    <input
      v-model="dniFiltro"
      type="text"
      placeholder="DNI"
      class="form-control mb-2"
    />

    <!-- Advertencia -->
    <div v-if="mostrarAdvertencia" class="alert alert-warning" role="alert">
      Ingrese al menos 3 caracteres en los filtros para realizar la búsqueda.
    </div>

    <ul v-if="resultados.length">
      <li v-for="p in resultados" :key="p.dni">
        {{ p.nombre }} {{ p.apellido }} - DNI: {{ p.dni }}
      </li>
    </ul>
    <p v-else>No hay resultados</p>
  </div>
</template>
