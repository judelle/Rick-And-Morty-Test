<template>
  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
    <div v-for="character in characters" :key="character.id" class="bg-white dark:bg-gray-900 rounded-lg p-4 cursor-pointer hover:shadow-lg transition-shadow" @click="goToCharacter(character.id)">
      <img :src="character.image" :alt="character.name" class="w-full h-48 object-cover rounded-lg mb-4" />
      <h3 class="text-xl font-bold">{{ character.name }}</h3>
      <p>{{ character.species }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { useRouter } from 'vue-router';
import { Character } from '../types';

export default defineComponent({
  name: 'CharacterResults',
  props: {
    characters: {
      type: Array as () => Character[],
      required: true,
    },
  },
  setup() {
    const router = useRouter();

    function goToCharacter(id: number) {
      router.push({ name: 'CharacterDetails', params: { id } });
    }

    return {
      goToCharacter,
    };
  },
});
</script>
