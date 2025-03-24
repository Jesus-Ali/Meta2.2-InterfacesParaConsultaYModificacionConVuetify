<template>
    <v-card>
      <v-img :src="imagen" alt="Imagen de la tarjeta"></v-img>
      <v-card-title>{{ titulo }}</v-card-title>
      <v-card-text>{{ texto }}</v-card-text>
    </v-card>
  </template>
  
  <script>
  export default {
    props: {
      imagen: {
        type: String,
        required: true
      },
      titulo: {
        type: String,
        required: true
      },
      texto: {
        type: String,
        required: true
      }
    }
  };
  </script>