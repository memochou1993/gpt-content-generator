<script setup>
import ContentGenerator from './components/ContentGenerator.vue';
</script>

<template>
  <v-app>
    <v-toolbar
      dark
      density="compact"
    >
      <v-toolbar-title>GPT Copywriter</v-toolbar-title>
    </v-toolbar>
    <v-main class="d-flex align-center bg-blue-grey-lighten-1">
      <v-container>
        <ContentGenerator />
      </v-container>
    </v-main>
  </v-app>
</template>
