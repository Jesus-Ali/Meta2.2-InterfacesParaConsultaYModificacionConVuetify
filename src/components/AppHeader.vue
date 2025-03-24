<template>
    <v-parallax :src="imagen" height="200" class="header-parallax">
      <v-row align="center" justify="center" class="header-content">
        <v-col class="text-center">
          <h1 class="header-title">{{ titulo }}</h1>
        </v-col>
      </v-row>
    </v-parallax>
  </template>
  
  <script>
  export default {
    props: {
      imagen: {
        type: String,
        required: true,
      },
      titulo: {
        type: String,
        required: true,
      },
    },
  };
  </script>
  
  <style scoped>
  .header-parallax {
    height: 200px; /* Altura fija para el encabezado */
  }
  
  .header-content {
    height: 100%; /* Ocupa toda la altura del contenedor */
  }
  
  .header-title {
    font-size: 2rem; /* Tamaño de fuente */
    font-weight: bold; /* Negrita */
    color: white; /* Color del texto */
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); /* Sombra del texto */
    margin: 0; /* Elimina el margen predeterminado */
  }
  </style>