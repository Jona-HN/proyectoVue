<template>
  <v-app>
    <custom-bar />

    <default-view />
  </v-app>
</template>

<script setup>
  import DefaultBar from './AppBar.vue'
  import DefaultView from './View.vue'
  import CustomBar from './CustomAppBar.vue'
</script>
