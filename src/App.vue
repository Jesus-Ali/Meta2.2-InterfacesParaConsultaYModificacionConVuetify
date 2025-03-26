<template>
  <v-app>
    <!-- Barra de navegación con indicador de carga -->
    <v-app-bar app color="primary" dark>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Admin Dashboard</v-toolbar-title>
      
      <!-- Barra de progreso para cargas generales -->
      <v-progress-linear
        v-if="isLoading"
        indeterminate
        absolute
        bottom
        color="white"
      ></v-progress-linear>
    </v-app-bar>

    <!-- Barra lateral (sin cambios) -->
    <v-navigation-drawer v-model="drawer" app temporary>
      <v-list>
        <v-list-item v-for="item in menuItems" :key="item.title" :to="item.route">
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <!-- Contenido principal con manejo de estados -->
    <v-main>
      <!-- Overlay de carga circular -->
      <v-overlay
        :value="isLoading"
        z-index="100"
        opacity="0.8"
      >
        <div class="text-center">
          <v-progress-circular
            indeterminate
            size="64"
            color="primary"
          ></v-progress-circular>
          <div class="mt-4 subtitle-1 white--text">Cargando...</div>
        </div>
      </v-overlay>

      <!-- Alertas globales -->
      <v-alert
        v-if="errorMessage"
        dense
        outlined
        type="error"
        class="ma-4"
        transition="scale-transition"
      >
        {{ errorMessage }}
        <v-btn
          icon
          small
          @click="errorMessage = null"
          class="float-right"
        >
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </v-alert>

      <!-- Contenido de la aplicación -->
      <router-view></router-view>
    </v-main>
  </v-app>
  
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
      isLoading: false,
      errorMessage: null,
      menuItems: [
        { title: 'Inicio', icon: 'mdi-home', route: '/' },
        { title: 'Usuarios', icon: 'mdi-account', route: '/users' },
        { title: 'Configuración', icon: 'mdi-cog', route: '/settings' },
      ],
    };
  },
  methods: {
    startLoading() {
      this.isLoading = true;
    },
    stopLoading() {
      this.isLoading = false;
    },
    showError(message) {
      this.errorMessage = message;
    },
    clearError() {
      this.errorMessage = null;
    }
  },
  provide() {
    return {
      appMethods: {
        startLoading: this.startLoading,
        stopLoading: this.stopLoading,
        showError: this.showError,
        clearError: this.clearError
      }
    };
  }
};

</script>

<style>
/* Estilos para el alerta fijo */
.v-alert {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 100;
  max-width: 400px;
}

/* Asegura que el overlay de carga cubra todo */
.v-overlay {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>