<script setup>
import {ref} from 'vue';
import axios from 'axios';
import MovieCard from './components/MovieCard.vue';
import MovieModal from './components/MovieModal.vue';

const API_KEY= '878eeed4';

const busqueda= ref('');
const resultados= ref([]);
const loading= ref(false);
const error= ref('');

const buscarPeliculas= async()=>{
  if(!busqueda.value) return
  loading.value= true
  error.value= ''

  try {
    const response= await axios.get(`https://www.omdbapi.com/?apikey=${API_KEY}&s=${busqueda.value}`)
    if(response.data.Response === "True"){
      resultados.value= response.data.Search
    }else{
      resultados.value= []
      
    }
  } catch(err){
    error.value= "Error al conectar con la API"
  } finally{
    loading.value= false
  }
}

const peliculaSeleccionada = ref(null)
const mostrarModal = ref(false)

const abrirModal = async (pelicula) => {
  mostrarModal.value = true
  peliculaSeleccionada.value = null

  try {
    const response= await axios.get(`https://www.omdbapi.com/?apikey=${API_KEY}&i=${pelicula.imdbID}`);
     peliculaSeleccionada.value= response.data
  } catch (error){
    console.error(error)
  } 
}

const cerrarModal = () => {
  mostrarModal.value = false
}
</script>

<template>
  <div class="app">
    <section class="hero">
      <div class="hero__overlay"></div>
      <div class="hero__content container text-center">
        <h1 class="hero__title">🎬 Encuentra tu próxima película</h1>
        <p class="hero__subtitle">Busca entre miles de películas y descubre nuevas historias</p>
        <div class="hero__search">
          <input 
            v-model="busqueda"
            @keyup.enter="buscarPeliculas"
            class="hero__input"
            placeholder="Buscar películas..."
          />
          <button class="hero__btn" @click="buscarPeliculas">
            Buscar
          </button>
        </div>
      </div>
    </section>
    <main class="app__container py-5">
      <div v-if="loading" class="movies__state movies__state--loading">
        <div class="spinner-border" role="status"></div>
        <p>Cargando películas...</p>
      </div>
      <div v-else-if="error" class="movies__state movies__state--error">
        <p>😕 {{ error }}</p>
      </div>
      <div v-else>
        <div v-if="resultados.length" class="row g-4">
          <div
            class="col-12 col-md-6 col-lg-3"
            v-for="pelicula in resultados"
            :key="pelicula.imdbID"
          >
            <MovieCard
              :titulo="pelicula.Title"
              :year="pelicula.Year"
              :poster="pelicula.Poster"
              @ver-detalle="abrirModal(pelicula)"
            />
            <MovieModal
              :pelicula="peliculaSeleccionada"
              :visible="mostrarModal"
              @close="cerrarModal"
            />
          </div>
        </div>
        <div v-else class="movies__state">
          <p>🎬 No se encontraron películas</p>
        </div>
      </div>      
    </main> 
  </div>
</template>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  width: 100%;
  min-height: 100%;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f8f9fa; 
}

.app {
  width: 100%;
  min-height: 100vh;
}

.app__container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem 1rem;
  margin-top: 3rem;
}

.hero {
  position: relative;
  height: 80vh;
  background-image: url('https://images.unsplash.com/photo-1524985069026-dd778a71c7b4');
  background-size: cover;
  background-position: center;
  display: flex;
  align-items: center;
  justify-content: center;
}

.hero__overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.6);
}

.hero__content {
  position: relative;
  z-index: 2;
  color: white;
  max-width: 700px;
  animation: fadeIn 1s ease;
}

.hero__title {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
}

.hero__subtitle {
  font-size: 1.1rem;
  margin-bottom: 2rem;
  color: #ddd;
}

.hero__search {
  display: flex;
  gap: 0.5rem;
  background: white;
  border-radius: 50px;
  padding: 0.5rem;
  overflow: hidden;
}

.hero__input {
  flex: 1;
  border: none;
  padding: 0.8rem 1rem;
  outline: none;
}

.hero__btn {
  background: #212529;
  color: white;
  border: none;
  padding: 0.8rem 1.5rem;
  border-radius: 50px;
  transition: 0.3s;
}

.hero__btn:hover {
  background: #000;
}

.movies__state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 300px;
  text-align: center;
  color: #6c757d;
}

.movies__state--loading {
  gap: 1rem;
  font-size: 1.1rem;
}

.movies__state--error {
  color: #dc3545;
  font-weight: 500;
  font-size: 1.1rem;
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