<template>
  <div class="p-4 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
    <div
      v-for="character in characters"
      :key="character.id"
      class="border p-4 rounded shadow"
    >
      <h2 class="text-xl font-bold">{{ character.name }}</h2>
      <p>Статус: {{ character.status }}</p>
      <p>Раса: {{ character.species }}</p>
      <p>Эпизоды: {{ character.episode.length }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'CharacterResults',
  props: {
    characters: {
      type: Array,
      required: true,
    },
  },
});
</script>
