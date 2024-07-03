<template>
  <div class="p-4">
    <div class="flex mb-2 space-x-4">
      <label class="flex-1">
        Жив?
        <select v-model="status" class="w-full p-2 border border-gray-300 rounded">
          <option value="">Все</option>
          <option value="alive">Жив</option>
          <option value="dead">Мертв</option>
          <option value="unknown">Неизвестно</option>
        </select>
      </label>
      <label class="flex-1">
        Раса
        <select v-model="species" class="w-full p-2 border border-gray-300 rounded">
          <option value="">Все</option>
          <option value="human">Человек</option>
          <option value="alien">Инопланетянин</option>
          <!-- Добавь другие расы по необходимости -->
        </select>
      </label>
    </div>
    <label class="block mb-2">
      Эпизод
      <input
        v-model="episode"
        type="text"
        placeholder="Введите эпизод"
        class="w-full p-2 border border-gray-300 rounded"
      />
    </label>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'CharacterFilters',
  data() {
    return {
      status: '',
      species: '',
      episode: '',
    };
  },
  watch: {
    status() {
      this.$emit('filter', { status: this.status, species: this.species, episode: this.episode });
    },
    species() {
      this.$emit('filter', { status: this.status, species: this.species, episode: this.episode });
    },
    episode() {
      this.$emit('filter', { status: this.status, species: this.species, episode: this.episode });
    },
  },
});
</script>
