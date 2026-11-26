<script setup lang="ts">
import type { Movie } from "../types";

defineProps<{ movie: Movie }>();
defineEmits<{ close: [] }>();
</script>

<template>
  <div class="overlay" @click.self="$emit('close')">
    <div class="modal">
      <button class="close" @click="$emit('close')">✕</button>
      <img
        v-if="movie.poster_path"
        :src="`https://image.tmdb.org/t/p/w300${movie.poster_path}`"
        :alt="movie.title"
      />
      <div>
        <h2>{{ movie.title }}</h2>
        <p>📅 {{ movie.release_date }}</p>
        <p>⭐ {{ movie.vote_average.toFixed(1) }}</p>
        <p>{{ movie.overview || "Sin sinopsis disponible." }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal {
  background: white;
  color: #222;
  padding: 1.5rem;
  border-radius: 12px;
  max-width: 600px;
  display: flex;
  gap: 1rem;
  position: relative;
}
.modal img {
  width: 180px;
  border-radius: 8px;
}
.close {
  position: absolute;
  top: 8px;
  right: 8px;
}
</style>
