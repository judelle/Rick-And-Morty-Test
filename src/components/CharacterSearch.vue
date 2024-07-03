<template>
  <div class="p-4">
    <input
      v-model="query"
      @keyup.enter="search"
      type="text"
      placeholder="Введите имя персонажа"
      class="w-full p-2 border border-gray-300 rounded"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'CharacterSearch',
  data() {
    return {
      query: '',
    };
  },
  methods: {
    search() {
      this.$emit('search', this.query);
    },
  },
});
</script>
