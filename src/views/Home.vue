<!-- Home.vue -->
<template>
  <v-app>
    <AppHeader imagen="https://picsum.photos/1600/400" titulo="Bienvenido a Mi Sitio Web" />
    <v-container>
      <v-row>
        <v-col cols="12" md="6">
          <TarjetaConImagen
            imagen="https://picsum.photos/500"
            titulo="Título de la Tarjeta 1"
            texto="Este es un texto descriptivo para la tarjeta 1. Puedes agregar cualquier contenido que desees aquí."
          />
        </v-col>
        <v-col cols="12" md="6">
          <TarjetaConImagen
            imagen="https://picsum.photos/501"
            titulo="Título de la Tarjeta 2"
            texto="Este es un texto descriptivo para la tarjeta 2. Puedes agregar cualquier contenido que desees aquí."
          />
        </v-col>
      </v-row>
    </v-container>
    <TablaDeDatos />
    <AppFooter texto="© 2024 Mi Sitio Web. Todos los derechos reservados." />
  </v-app>
</template>

<script>
import AppHeader from '../components/AppHeader.vue';
import TarjetaConImagen from '../components/TarjetaConImagen.vue';
import TablaDeDatos from '../components/TablaDeDatos.vue';
import AppFooter from '../components/AppFooter.vue';

export default {
  name: 'HomeView',
  components: {
    AppHeader,
    TarjetaConImagen,
    TablaDeDatos,
    AppFooter
  }
};
</script>