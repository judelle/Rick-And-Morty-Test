<template>
  <div v-if="character" class="p-4 flex flex-col items-center">
    <h1 class="text-3xl font-bold mb-4">{{ character.name }}</h1>
    <img :src="character.image" :alt="character.name" class="w-full max-w-2xl h-auto object-contain rounded-lg mb-4" />
    <div class="text-left w-full max-w-2xl">
      <p><strong>Статус:</strong> {{ character.status }}</p>
      <p><strong>Раса:</strong> {{ character.species }}</p>
      <p><strong>Пол:</strong> {{ character.gender }}</p>
      <p><strong>Происхождение:</strong> {{ character.origin.name }}</p>
      <p><strong>Локация:</strong> {{ character.location.name }}</p>
      <p><strong>Эпизоды:</strong></p>
      <ul class="list-disc pl-5">
        <p v-for="episode in episodes" :key="episode.id">{{ episode.name }} ({{ episode.episode }})</p>
      </ul>
    </div>
  </div>
  <div v-else-if="error" class="text-red-500">{{ error }}</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';
import { Character } from '../types';

interface Episode {
  id: number;
  name: string;
  episode: string;
}

export default defineComponent({
  name: 'CharacterDetails',
  data() {
    return {
      character: null as Character | null,
      episodes: [] as Episode[],
      error: null as string | null,
    };
  },
  async created() {
    const route = useRoute();
    const characterId = route.params.id as string;

    try {
      const response = await axios.get(`https://rickandmortyapi.com/api/character/${characterId}`);
      this.character = response.data;

      const episodeUrls = this.character?.episode || [];
      const episodeRequests = episodeUrls.map(url => axios.get(url));
      const episodeResponses = await Promise.all(episodeRequests);

      this.episodes = episodeResponses.map(response => response.data);
    } catch (error) {
      this.error = 'Что-то пошло не так. Попробуйте позже.';
    }
  },
});
</script>