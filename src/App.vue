<script setup>
import ContentGenerator from './components/ContentGenerator.vue';
</script>

<template>
  <v-app>
    <v-app-bar density="compact" elevation="2">
      <v-app-bar-title>AI 內容生成器</v-app-bar-title>
    </v-app-bar>
    <v-main class="d-flex align-center bg-blue-grey-lighten-1">
      <v-container class="py-16">
        <ContentGenerator />
      </v-container>
    </v-main>
    <v-footer color="blue-grey-lighten-5">
      <v-row no-gutters>
        <v-col class="text-center">
          Powered by
          <span class="font-weight-medium">
            <a href="https://github.com/memochou1993/gpt-content-generator" target="_blank" rel="noopener noreferrer">
              GPT Content Generator
            </a>
          </span>
        </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>
