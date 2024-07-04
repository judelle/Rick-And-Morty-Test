<template>
    <div class="container mx-auto p-4">
      <ul class="list-disc pl-4 mb-4">
        <p>Frontend разработчик - Лумпов Александр Артемовчи</p>
      </ul>
      <h2 class="text-xl font-bold mb-2">Технологии</h2>
      <p>
        Я использую современные технологии для разработки данного проекта:
      </p>
      <ul class="list-disc pl-4">
        <p>Vue.js - основной фреймворк</p>
        <p>Tailwind CSS - для стилизации</p>
        <p>TypeScript - для улучшения типизации и поддержки</p>
        <p>Vue Router - для маршрутизации</p>
      </ul>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    name: 'AboutPage',
  });
  </script>
  
  <style scoped>
  </style>
  