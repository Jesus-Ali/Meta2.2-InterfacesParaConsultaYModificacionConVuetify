<template>
    <v-footer class="custom-footer">
      <v-col class="text-center white--text" cols="12">
        {{ texto }}
      </v-col>
    </v-footer>
  </template>
  
  <script>
  export default {
    props: {
      texto: {
        type: String,
        required: true,
      },
    },
  };
  </script>
  
  <style scoped>
  .custom-footer {
    background-color: #222; /* Fondo oscuro */
    color: white; /* Color de texto blanco */
    padding: 10px 0; /* Padding */
    margin-top: 20px; /* Margen superior */
  }
  </style>