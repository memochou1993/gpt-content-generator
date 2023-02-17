<script setup>
import ContentGenerator from './components/ContentGenerator.vue';
</script>

<template>
  <v-app>
    <v-toolbar density="compact">
      <v-toolbar-title>AI 文案生成器</v-toolbar-title>
    </v-toolbar>
    <v-main class="d-flex align-center bg-blue-grey-lighten-1 py-8">
      <v-container>
        <ContentGenerator />
      </v-container>
    </v-main>
    <v-footer color="blue-grey-lighten-5">
      <v-row>
        <v-col class="text-center">
          Powered by gpt-copywriter
        </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>
