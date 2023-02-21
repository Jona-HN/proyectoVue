<template>
  <v-app-bar
    color="primary"
    prominent>
    <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer" />
    <v-toolbar-title>
      Mi primera app en Vue
    </v-toolbar-title>

    <!-- <v-spacer />
    
    <v-btn variant="text" icon="mdi-magnify" />

    <v-btn variant="text" icon="mdi-filter" />

    <v-btn variant="text" icon="mdi-dots-vertical" /> -->
  </v-app-bar>

  <v-navigation-drawer
    v-model="drawer"
    location="left"
    temporary
  >
    <v-list-item
      title="Opciones"
    ></v-list-item>

    <v-divider></v-divider>

    <v-list density="compact" nav>
      <v-list-item @click="$router.push('/')"
        prepend-icon="mdi-view-dashboard"
        title="Home"
        value="value"
      ></v-list-item>

      <v-list-item @click="$router.push('tabla')"
        prepend-icon="mdi-table"
        title="Tabla"
        value="value"
      ></v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script setup>
  import { ref } from 'vue';

  const drawer = ref(false);
</script>
