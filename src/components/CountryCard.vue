<script>
export default {
  props: {
    pais: Object
  }
}
</script>

<template>
<div class="card">

  <img :src="pais.flags?.png" alt="bandera">

  <h2>{{ pais.name?.common }}</h2>

  <p><b>Capital:</b> {{ pais.capital ? pais.capital[0] : "Sin capital" }}</p>

  <p><b>Región:</b> {{ pais.region }}</p>

  <p><b>Población:</b> {{ pais.population?.toLocaleString() }}</p>

</div>
</template>

<style>
.card{
  border:1px solid #ddd;
  border-radius:10px;
  padding:15px;
  background:white;
  box-shadow:0 2px 5px rgba(0,0,0,0.1);
}

.card img{
  width:100%;
  height:140px;
  object-fit:cover;
  border-radius:6px;
}
</style>