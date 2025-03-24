<template>
    <v-container>
      <!-- Tabla de datos -->
      <v-data-table
        :headers="headers"
        :items="items"
        hide-default-footer
        class="elevation-1"
      ></v-data-table>
  
      <!-- Botón "Enviar" centrado -->
      <div class="text-center mt-4">
        <v-btn color="success" @click="enviar">Enviar</v-btn>
      </div>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
      return {
        headers: [
          { text: 'Columna 1', value: 'col1' },
          { text: 'Columna 2', value: 'col2' },
          { text: 'Columna 3', value: 'col3' },
        ],
        items: [
          { col1: 'Valor 1', col2: 'Valor 2', col3: 'Valor 3' },
          { col1: 'Valor 4', col2: 'Valor 5', col3: 'Valor 6' },
        ],
      };
    },
    methods: {
      enviar() {
        window.location.href = 'https://youtu.be/dQw4w9WgXcQ';
      },
    },
  };
  </script>