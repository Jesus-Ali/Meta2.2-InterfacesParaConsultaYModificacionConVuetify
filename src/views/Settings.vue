<template>
  <v-container>
    <h1>Configuración</h1>
    <p>Esta es la página de configuraciones.</p>
    
    <!-- GIF desde assets locales -->
    <v-img
      :src="require('../assets/cockroach-spinning.gif')"
      alt="Animación de configuración"
      contain
      max-height="300"
      class="mt-4"
    ></v-img>
  </v-container>
</template>

<script>
export default {
  name: 'SettingsView',
};
</script>