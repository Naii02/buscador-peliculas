<script setup lang="ts">
import { ref } from "vue";
import type { Movie } from "./types";

const query = ref("");
const movies = ref<Movie[]>([]);
const loading = ref(false);
const error = ref("");

const API_KEY = import.meta.env.VITE_TMDB_KEY;

async function search() {
  if (!query.value.trim()) return;
  loading.value = true;
  error.value = "";
  try {
    const res = await fetch(
      `https://api.themoviedb.org/3/search/movie?api_key=${API_KEY}&language=es-MX&query=${encodeURIComponent(query.value)}`,
    );
    if (!res.ok) throw new Error("Error");
    const data = await res.json();
    movies.value = data.results;
  } catch {
    error.value = "No se pudo buscar. Intenta de nuevo.";
  } finally {
    loading.value = false;
  }
}
</script>

<template>
  <main>
    <h1>🎬 Buscador de películas</h1>

    <input
      v-model="query"
      placeholder="Busca una película..."
      @keyup.enter="search"
    />
    <button @click="search">Buscar</button>

    <p v-if="loading">Cargando...</p>
    <p v-if="error">{{ error }}</p>

    <div class="grid">
      <div v-for="movie in movies" :key="movie.id" class="card">
        <img
          v-if="movie.poster_path"
          :src="`https://image.tmdb.org/t/p/w300${movie.poster_path}`"
          :alt="movie.title"
        />
        <h3>{{ movie.title }}</h3>
        <p>⭐ {{ movie.vote_average.toFixed(1) }}</p>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
}
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
  gap: 1rem;
  margin-top: 1.5rem;
}
.card img {
  width: 100%;
  border-radius: 8px;
}
</style>
