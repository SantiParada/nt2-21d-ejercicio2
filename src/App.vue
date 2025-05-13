<script setup>
import { ref, computed } from 'vue'

const filtroNombre = ref('')
const filtroDni = ref('')
const personas = ref([
  { nombre: 'Juan', apellido: 'Pérez', dni: '12345678' },
  { nombre: 'Ana', apellido: 'Gómez', dni: '87654321' },
  { nombre: 'Lucas', apellido: 'Fernández', dni: '45678901' },
  { nombre: 'Santino', apellido: 'Parada', dni: '43626528' }
])

const mostrarAlerta = computed(() => {
  return (
    (filtroNombre.value && filtroNombre.value.length < 3) ||
    (filtroDni.value && filtroDni.value.length < 3)
  )
})

const personasFiltradas = computed(() => {
  const nombreActivo = filtroNombre.value.trim().length >= 3
  const dniActivo = filtroDni.value.trim().length >= 3

  if (!nombreActivo && !dniActivo) return []

  return personas.value.filter(p => {
    const nombreCompleto = (p.nombre + ' ' + p.apellido).toLowerCase()
    const nombreMatch = nombreCompleto.includes(filtroNombre.value.toLowerCase())
    const dniMatch = p.dni.includes(filtroDni.value)

    if (nombreActivo && dniActivo) return nombreMatch && dniMatch
    if (nombreActivo) return nombreMatch
    if (dniActivo) return dniMatch
  })
})
</script>

<template>
  <div class="container mt-4">
    <h2>Buscar Personas</h2>
    <input v-model="filtroNombre" class="form-control mb-2" placeholder="Nombre o apellido" />
    <input v-model="filtroDni" class="form-control mb-2" placeholder="DNI" />

    <!-- Alert -->
    <div v-if="mostrarAlerta" class="alert alert-warning" role="alert">
      Ingresá al menos 3 caracteres en alguno de los filtros para buscar.
    </div>

    <!-- Lista -->
    <ul v-if="personasFiltradas.length">
      <li v-for="p in personasFiltradas" :key="p.dni">
        {{ p.nombre }} {{ p.apellido }} - DNI: {{ p.dni }}
      </li>
    </ul>
    <p v-else>No hay resultados o los filtros son insuficientes.</p>
  </div>
</template>


