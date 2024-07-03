<template>
  <div :class="['container mx-auto p-4', themeClass]">
    <div class="flex justify-end mb-4">
      <label for="theme-select" class="mr-2">Тема:</label>
      <select id="theme-select" @change="changeTheme" class="p-2 border border-gray-300 rounded">
        <option value="light" :selected="!isDarkMode && !isRickAndMortyTheme">Светлая тема</option>
        <option value="dark" :selected="isDarkMode">Темная тема</option>
        <option value="rick-and-morty" :selected="isRickAndMortyTheme">Тематическая тема</option>
      </select>
    </div>
    <CharacterSearch @search="searchCharacters" />
    <CharacterFilters @filter="applyFilters" />
    <CharacterResults :characters="filteredCharacters" />
    <p v-if="errorMessage" class="text-red-500 mt-4">{{ errorMessage }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import CharacterSearch from './components/CharacterSearch.vue';
import CharacterFilters from './components/CharacterFilters.vue';
import CharacterResults from './components/CharacterResults.vue';
import axios from 'axios';

export default defineComponent({
  components: {
    CharacterSearch,
    CharacterFilters,
    CharacterResults,
  },
  data() {
    return {
      isDarkMode: false as boolean,
      isRickAndMortyTheme: false as boolean,
      characters: [] as any[],
      filteredCharacters: [] as any[],
      filters: {
        name: '',
        status: '',
        species: '',
        episode: '',
      },
      errorMessage: '',
    };
  },
  computed: {
    themeClass(): string {
      if (this.isRickAndMortyTheme) {
        return 'rick-and-morty-theme';
      } else if (this.isDarkMode) {
        return 'dark';
      }
      return '';
    }
  },
  async created() {
    this.loadFilters();
    this.isDarkMode = localStorage.getItem('darkMode') === 'true';
    this.isRickAndMortyTheme = localStorage.getItem('rickAndMortyTheme') === 'true';
    if (this.isDarkMode) {
      document.documentElement.classList.add('dark');
    }
    if (this.isRickAndMortyTheme) {
      document.documentElement.classList.add('rick-and-morty-theme');
    }
    if (this.filters.name) {
      await this.fetchCharacters();
    }
  },
  methods: {
    changeTheme(event: Event) {
      const selectedTheme = (event.target as HTMLSelectElement).value;
      if (selectedTheme === 'dark') {
        this.isDarkMode = true;
        this.isRickAndMortyTheme = false;
      } else if (selectedTheme === 'rick-and-morty') {
        this.isDarkMode = false;
        this.isRickAndMortyTheme = true;
      } else {
        this.isDarkMode = false;
        this.isRickAndMortyTheme = false;
      }
      this.saveThemePreferences();
    },
    saveThemePreferences() {
      localStorage.setItem('darkMode', this.isDarkMode.toString());
      localStorage.setItem('rickAndMortyTheme', this.isRickAndMortyTheme.toString());
      if (this.isDarkMode) {
        document.documentElement.classList.add('dark');
        document.documentElement.classList.remove('rick-and-morty-theme');
      } else if (this.isRickAndMortyTheme) {
        document.documentElement.classList.add('rick-and-morty-theme');
        document.documentElement.classList.remove('dark');
      } else {
        document.documentElement.classList.remove('dark');
        document.documentElement.classList.remove('rick-and-morty-theme');
      }
    },
    saveFilters() {
      localStorage.setItem('filters', JSON.stringify(this.filters));
    },
    loadFilters() {
      const savedFilters = localStorage.getItem('filters');
      if (savedFilters) {
        this.filters = JSON.parse(savedFilters);
      }
    },
    async searchCharacters(name: string) {
      this.filters.name = name;
      this.saveFilters();
      await this.fetchCharacters();
    },
    async applyFilters(filters: { status: string; species: string; episode: string }) {
      this.filters.status = filters.status;
      this.filters.species = filters.species;
      this.filters.episode = filters.episode;
      this.saveFilters();
      this.filteredCharacters = this.characters.filter((character: any) => {
        return (
          (!filters.status || character.status.toLowerCase() === filters.status) &&
          (!filters.species || character.species.toLowerCase() === filters.species) &&
          (!filters.episode || character.episode.some((ep: string) => ep.includes(filters.episode)))
        );
      });
    },
    async fetchCharacters() {
      try {
        this.errorMessage = '';
        const response = await axios.get(`https://rickandmortyapi.com/api/character?name=${this.filters.name}`);
        this.characters = response.data.results;
        this.applyFilters(this.filters);
      } catch (error) {
        this.errorMessage = 'Что-то пошло не так. Попробуйте еще раз.';
        console.error(error);
      }
    },
  },
});
</script>

<style>
html {
  transition: background-color 0.5s ease, color 0.5s ease;
}

html.dark {
  background-color: #1a202c;
  color: #a0aec0;
  transition: background-color 0.5s ease, color 0.5s ease;
}

html.dark select,
html.dark input,
html.dark option {
  background-color: #2d3748;
  color: #a0aec0;
}

html.rick-and-morty-theme {
  background-color: #f0f8ff;
  color: #00a8e8;
  font-family: 'Comic Sans MS', cursive, sans-serif;
  transition: background-color 0.5s ease, color 0.5s ease;
}

html.rick-and-morty-theme select,
html.rick-and-morty-theme input,
html.rick-and-morty-theme option {
  background-color: #c4f0c5;
  color: #00a8e8;
  border-color: #00a8e8;
}
</style>
