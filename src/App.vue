<script setup>
import { ref, onMounted, computed } from "vue"
import CountryCard from "./components/CountryCard.vue"

const paises = ref([])
const cargando = ref(true)
const busqueda = ref("")
const region = ref("")

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

const paisesFiltrados = computed(() => {
  return paises.value.filter(p =>
    p.name.common.toLowerCase().includes(busqueda.value.toLowerCase()) ||
    (p.region && p.region.toLowerCase().includes(busqueda.value.toLowerCase()))
  )
})
</script>

<template>
  <div class="container">

    <h1>🌎 Explorador de Países</h1>

    <input
      v-model="busqueda"
      placeholder="Buscar país..."
      class="search"
    />

    <p v-if="cargando">⏳ Cargando países...</p>

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

<style>

.container{
  max-width:1200px;
  margin:auto;
  text-align:center;
}

.search{
  padding:10px;
  width:300px;
  margin:20px;
}

.grid{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(250px,1fr));
  gap:20px;
}

</style>
