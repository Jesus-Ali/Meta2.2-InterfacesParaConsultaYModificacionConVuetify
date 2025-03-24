<template>
  <v-app>
    <!-- Barra de navegación -->
    <v-app-bar app color="primary" dark>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Admin Dashboard</v-toolbar-title>
    </v-app-bar>

    <!-- Barra lateral -->
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

    <!-- Contenido principal -->
    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
      menuItems: [
        { title: 'Inicio', icon: 'mdi-home', route: '/' },
        { title: 'Usuarios', icon: 'mdi-account', route: '/users' },
        { title: 'Configuración', icon: 'mdi-cog', route: '/settings' },
      ],
    };
  },
};
</script>

<style>
/* Estilos personalizados si es necesario */
</style>