<script setup>
import { ref, computed } from 'vue'

const filtroNombre = ref('')
const filtroDni = ref('')

const personas = ref([
  { nombre: 'Ana', apellido: 'Gómez', dni: '12345678' },
  { nombre: 'Carlos', apellido: 'Pérez', dni: '87654321' },
  { nombre: 'Lucía', apellido: 'Martínez', dni: '55554444' },
])

const personasFiltradas = computed(() =>
  personas.value.filter(persona => {
    const nombreCompleto = `${persona.nombre} ${persona.apellido}`.toLowerCase()
    return (
      nombreCompleto.includes(filtroNombre.value.toLowerCase()) &&
      persona.dni.includes(filtroDni.value)
    )
  })
)
</script>

<template>
  <div>
    <h1>Buscar Personas</h1>

    <input v-model="filtroNombre" placeholder="Buscar por nombre o apellido" class="input" />
    <input v-model="filtroDni" placeholder="Buscar por DNI" class="input" />

    <ul>
      <li v-for="persona in personasFiltradas" :key="persona.dni">
        {{ persona.nombre }} {{ persona.apellido }} - DNI: {{ persona.dni }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.input {
  display: block;
  margin: 0.5rem 0;
  padding: 0.5rem;
  font-size: 1rem;
}
</style>
