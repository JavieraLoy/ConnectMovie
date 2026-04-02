<template>
  <div v-if="visible" class="movie-modal">
    <div class="movie-modal__overlay" @click="emit('close')"></div>
    <div class="movie-modal__content">
      <button class="movie-modal__close" @click="emit('close')">
        ✕
      </button>
      <div v-if="!pelicula" class="movie-modal__loading">
        <div class="spinner-border"></div>
        <p>Cargando detalles...</p>
      </div>
      <div v-else class="row g-4">
        <div class="col-12 col-md-4">
            <div class="movie-modal__img-container">
                <img 
                  v-if="pelicula.Poster !== 'N/A' && !imgError"
                  :src="pelicula.Poster"
                  class="movie-modal__img"
                  @error="imgError= true"
                />
                <div v-else class="movie-modal__placeholder">
                    🎬<span>{{ pelicula.Title }}</span>
                </div>
            </div>
        </div>
        <div class="col-12 col-md-8">
          <h2 class="movie-modal__title">{{ pelicula.Title }}</h2>
          <p class="movie-modal__meta">{{ pelicula.Year }} • {{ pelicula.Genre }}</p>
          <p class="movie-modal__rating">⭐ {{ pelicula.imdbRating }}</p>
          <p class="movie-modal__plot">{{ pelicula.Plot }}</p>
          <p class="movie-modal__extra"><strong>Actores:</strong> {{ pelicula.Actors }}</p>
          <p class="movie-modal__extra"><strong>Director:</strong> {{ pelicula.Director }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import {ref, watch} from 'vue';

const props= defineProps({
  pelicula: Object,
  visible: Boolean
})

const emit = defineEmits(['close'])
const imgError= ref(false)

watch(() => props.pelicula, () => {
  imgError.value = false
})

</script>

<style scoped>
.movie-modal {
  position: fixed;
  inset: 0;
  z-index: 999;
}

.movie-modal__overlay {
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.7);
}

.movie-modal__content {
  position: relative;
  max-width: 800px;
  margin: 5% auto;
  background: white;
  border-radius: 12px;
  padding: 2rem;
  z-index: 2;
  animation: fadeIn 0.3s ease;
}

.movie-modal__close {
  position: absolute;
  top: 10px;
  right: 10px;
  border: none;
  background: #212529;
  color: white;
  border-radius: 50%;
  width: 35px;
  height: 35px;
}

.movie-modal__loading {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 300px;
  gap: 1rem;
}

.movie-modal__img-container {
  width: 100%;
}
.movie-modal__img {
  width: 100%;
  border-radius: 10px;
}

.movie-modal__placeholder {
  width: 100%;
  height: 400px;
  background: linear-gradient(135deg, #2c2c2c, #1a1a1a);
  color: #ccc;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
}

.movie-modal__title {
  font-weight: 700;
}

.movie-modal__meta {
  color: #6c757d;
  margin-bottom: 0.5rem;
}

.movie-modal__rating {
  font-weight: 600;
  margin-bottom: 1rem;
}

.movie-modal__plot {
  margin-bottom: 1rem;
  line-height: 1.5;
}

.movie-modal__extra {
  font-size: 0.9rem;
  color: #444;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>