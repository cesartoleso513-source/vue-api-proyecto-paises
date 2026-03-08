<template>
  <div class="card">
    <div class="card-image-container">
      <img :src="pais.flags?.png" :alt="`Bandera de ${pais.name?.common}`" class="card-img">
    </div>

    <div class="card-content">
      <h2 class="country-name">{{ pais.name?.common }}</h2>
      
      <div class="country-info">
        <p><span>Capital:</span> {{ pais.capital ? pais.capital[0] : "Sin capital" }}</p>
        <p><span>Región:</span> {{ pais.region }}</p>
        <p><span>Población:</span> {{ pais.population?.toLocaleString() }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pais: Object
  }
}
</script>

<style scoped>
.card {
  background: white;
  border-radius: 20px; /* Bordes más redondeados estilo moderno */
  overflow: hidden;
  border: 1px solid #f0f0f0;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  cursor: pointer;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
}

/* Efecto de elevación al pasar el mouse */
.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

.card-image-container {
  width: 100%;
  height: 160px;
  overflow: hidden;
}

.card-img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Evita que las banderas se deformen */
  transition: transform 0.5s ease;
}

.card:hover .card-img {
  transform: scale(1.05); /* Zoom sutil en la bandera al hacer hover */
}

.card-content {
  padding: 20px;
  text-align: left;
}

.country-name {
  margin: 0 0 12px 0;
  font-size: 1.25rem;
  color: #1a1a1a;
  font-weight: 700;
}

.country-info p {
  margin: 6px 0;
  font-size: 0.9rem;
  color: #666; /* Texto secundario en gris */
}

.country-info span {
  font-weight: 600;
  color: #333; /* Etiquetas con más peso visual */
}
</style>
