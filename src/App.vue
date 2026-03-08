<script setup>
import { ref, onMounted, computed } from "vue"
import CountryCard from "./components/CountryCard.vue"

const paises = ref([])
const cargando = ref(true)
const busqueda = ref("")
const region = ref("") // Variable para el filtro de región

onMounted(async () => {
  try {
    const res = await fetch(
      "https://restcountries.com/v3.1/all?fields=name,flags,capital,region,subregion,population,languages,currencies"
    )
    paises.value = await res.json()
  } catch (error) {
    console.log("Error al cargar países", error)
  } finally {
    cargando.value = false
  }
})

// Lógica de filtrado combinada
const paisesFiltrados = computed(() => {
  return paises.value.filter(p => {
    // Filtro por nombre
    const coincideNombre = p.name.common.toLowerCase().includes(busqueda.value.toLowerCase())
    
    // Filtro por región (si region está vacío, muestra todos)
    const coincideRegion = region.value === "" || p.region === region.value
    
    return coincideNombre && coincideRegion
  })
})
</script>

<template>
  <div class="container">
    <h1>🌎 Explorador de Países</h1>

    <div class="controles">
      <input 
        v-model="busqueda" 
        placeholder="Buscar país..." 
        class="search"
      />

      <select v-model="region" class="region-filter">
        <option value="">Todas las regiones</option>
        <option value="Africa">África</option>
        <option value="Americas">América</option>
        <option value="Asia">Asia</option>
        <option value="Europe">Europa</option>
        <option value="Oceania">Oceanía</option>
      </select>
    </div>

    <p v-if="cargando">⌛ Cargando países...</p>

    <div v-else class="grid">
      <CountryCard 
        v-for="pais in paisesFiltrados" 
        :key="pais.name.common" 
        :pais="pais" 
      />
    </div>

    <p v-if="!cargando && paisesFiltrados.length === 0">
      No se encontraron países
    </p>
  </div>
</template>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px;
  text-align: center;
}

h1 {
  margin-bottom: 30px;
  font-size: 2.5rem;
  color: #1a1a1a;
}

.controles {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 40px;
  flex-wrap: wrap;
}

.search, .region-filter {
  padding: 12px 20px;
  border-radius: 12px;
  border: 1px solid #ddd;
  font-size: 1rem;
  outline: none;
  transition: all 0.3s ease;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
}

.search {
  flex: 1;
  max-width: 400px;
}

.region-filter {
  cursor: pointer;
  background-color: white;
}

.search:focus, .region-filter:focus {
  border-color: #007aff;
  box-shadow: 0 0 0 4px rgba(0, 122, 255, 0.1);
}

.grid {
  display: grid;
  /* Grid responsivo: ajusta columnas automáticamente según el ancho */
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 30px;
  padding: 20px 0;
}
</style>
