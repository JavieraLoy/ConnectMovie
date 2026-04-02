<template>
    <div class="movie-card card h-100 shadow-sm">
        <div class="movie-card__img-container">
            <img 
              v-if="poster !== 'N/A' && !imgError"
              :src="poster"
              class="movie-card__img"
              alt="poster"
              @error="imgError= true"
            />
            <div v-else class="movie-card__placeholder">
                🎬<span>{{ titulo }}</span>
            </div>
            <div class="movie-card__overlay">
                <button class="movie-card__btn" @click="emit('ver-detalle')">
                    Ver detalle
                </button>
            </div>
        </div>
        <div class="movie-card__body card-body">
            <h6 class="movie-card__title">{{ titulo }}</h6>
            <span class="movie-card__badge">{{ year }}</span>
        </div>
    </div>
</template>

<script setup>
import {ref} from 'vue';

defineProps({
  titulo: String,
  year: String,
  poster: String
});

const emit= defineEmits(['ver-detalle']);
const imgError= ref(false)
</script>

<style scoped>
.movie-card {
  border: none;
  border-radius: 12px;
  overflow: hidden;
  transition: transform 0.3s ease;
  position: relative;
}

.movie-card:hover {
  transform: translateY(-5px);
}

.movie-card__img-container {
  position: relative;
  width: 100%;
  aspect-ratio: 2 / 3;
  overflow: hidden;
}

.movie-card__img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.movie-card__placeholder {
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #2c2c2c, #1a1a1a);
  color: #ccc;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.movie-card__placeholder span {
  font-size: 0.9rem;
  margin-top: 0.5rem;
}

.movie-card__overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: 0.3s ease;
}

.movie-card:hover .movie-card__overlay {
  opacity: 1;
}

.movie-card__btn {
  background: white;
  border: none;
  padding: 0.6rem 1.2rem;
  border-radius: 30px;
  font-size: 0.9rem;
  transition: 0.3s;
}

.movie-card__btn:hover {
  background: #212529;
  color: white;
}

.movie-card__body {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.movie-card__title {
  font-size: 0.95rem;
  font-weight: 600;
  margin: 0;
  max-width: 70%;
}

.movie-card__badge {
  background: #212529;
  color: white;
  padding: 0.2rem 0.6rem;
  border-radius: 8px;
  font-size: 0.75rem;
}
</style>