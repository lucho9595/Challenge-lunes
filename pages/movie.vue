<template>
    <div class="container-fluid">
        <div class="movie-container">
            <h1 class="p">Películas</h1>
            <div class="search-container">
                <input type="text" v-model="searchQuery" @input="searchMovies" placeholder="Buscar película"
                    class="search-input" />
                <button @click="clearSearch" class="clear-button">Limpiar</button>
            </div>
            <div class="row">
                    <div v-for="movie in filteredMovies" :key="movie.imdbID" class="col-12 col-md-6 col-lg-4 movie-card">
                        <div class="card-image" @click="goToDetail(movie)">
                            <img v-if="movie.Poster !== 'N/A'" :src="movie.Poster" alt="Movie Poster"
                                class="card-img-top" />
                            <div v-else class="loading-img"></div>
                            <div class="card-overlay">
                                <span class="card-overlay-text">Click here</span>
                            </div>
                        </div>
                        <div class="card-body">
                            <h2 class="card-title">{{ movie.Title }}</h2>
                        </div>
                    </div>
            </div>
            <div class="pagination">
                <button @click="previousPage" :disabled="currentPage === 1" class="btn btn-primary">
                    Anterior
                </button>
                <span class="current-page">{{ currentPage }}</span>
                <button @click="nextPage" :disabled="currentPage === totalPages" class="btn btn-primary">
                    Siguiente
                </button>
            </div>
        </div>
    </div>
</template>


<script>
import axios from 'axios';

export default {
    name: 'movie',
    data() {
        return {
            movies: [], // Lista de películas
            currentPage: 1, // Página actual
            totalResults: 0, // Total de resultados
            totalPages: 0, // Total de páginas
            apiKey: '7e46d54c', // Reemplaza "tu-api-key" con tu propia clave de API
            searchQuery: '', // Consulta de búsqueda
            loading: false, // Estado de carga
        };
    },
    mounted() {
        this.fetchMovies();
    },
    methods: {
        fetchMovies() {
            const url = `https://www.omdbapi.com/?apikey=${this.apiKey}&s=movie&type=movie&page=${this.currentPage}`;
            this.loading = true; // Mostrar estado de carga
            axios
                .get(url)
                .then(response => {
                    this.movies = response.data.Search || [];
                    this.totalResults = parseInt(response.data.totalResults);
                    this.totalPages = Math.ceil(this.totalResults / 10);
                })
                .catch(error => {
                    console.error('Error al obtener las películas:', error);
                })
                .finally(() => {
                    this.loading = false; // Ocultar estado de carga
                });
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++;
                this.fetchMovies();
                this.searchMovies(); // Agregamos la llamada a searchMovies para mantener la búsqueda activa en la nueva página
            }
        },
        previousPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
                this.fetchMovies();
            }
        },
        goToDetail(movie) {
            this.$router.push(`/movies/${movie.imdbID}`);
        },
        searchMovies() {
            if (this.searchQuery === '') {
                // Si la consulta de búsqueda está vacía, muestra todas las películas
                this.fetchMovies();
            } else {
                // Realiza una búsqueda según la consulta
                const searchUrl = `https://www.omdbapi.com/?apikey=${this.apiKey}&s=${this.searchQuery}&type=movie`;
                axios
                    .get(searchUrl)
                    .then(response => {
                        this.movies = response.data.Search || [];
                        this.totalResults = parseInt(response.data.totalResults);
                        this.totalPages = Math.ceil(this.totalResults / 10);
                    })
                    .catch(error => {
                        console.error('Error al realizar la búsqueda:', error);
                    });
            }
        },
        clearSearch() {
            this.searchQuery = '';
            this.fetchMovies();
        },
    },
    computed: {
        // Utilizamos filteredMovies en lugar de movies en el bucle v-for
        filteredMovies() {
            // Filtra las películas según la consulta de búsqueda
            if (this.searchQuery === '') {
                return this.movies;
            } else {
                return this.movies.filter(movie =>
                    movie.Title.toLowerCase().includes(this.searchQuery.toLowerCase())
                );
            }
        },
    },
};
</script>
  
<style scoped>
.p {
  color: red;
  text-align: center;
  margin-bottom: 20px;
}

.p:hover {
  text-shadow: -3px 2px 50px white;
}

.container-fluid {
  background-color: black;
}

.search-container {
  display: flex;
  flex-direction: row;
  margin-bottom: 20px;
  justify-content: center;
  align-items: center;
}

.search-input {
  width: 200px;
  /* Ajusta el ancho según tus necesidades */
  padding: 8px;
  background: #222;
  /* Cambia el color de fondo del searchBar */
  outline: none;
  border-radius: 6px;
  font-size: 14px;
  /* Ajusta el tamaño del texto del searchBar */
  color: white;
  /* Cambia el color del texto del searchBar */
  border: none;
}

.clear-button {
  border: 0px solid white;
  background: red;
  border-radius: 6px;
  color: #fffdfd;
  font-size: 14px;
  /* Ajusta el tamaño del texto del botón Limpiar */
  cursor: pointer;
  transition: 0.3s;
  margin-left: 10px;
  padding: 6px 10px;
}

.clear-button:hover {
  background-color: rgb(94, 8, 8);
}

.movie-container {
  padding: 20px;
}

.movie-card {
  display: flex;
  margin-bottom: 20px;
  flex-direction: column;
  align-items: center;
}

.card-wrapper {
  position: relative;
  width: 200px;
  height: 300px;
  /* Ajusta el tamaño de las imágenes */
}

.card-img-top {
  width: 100%;
  height: 100%;
  object-fit: cover;
  cursor: pointer;
  transition: transform 0.3s ease;
  opacity: 0.8;
}

.card-img-top:hover {
  transform: scale(1.1);
  opacity: 1;
}

.card-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.card-wrapper:hover .card-overlay {
  opacity: 1;
}

.card-overlay-text {
  color: white;
  font-weight: bold;
}

.card-body {
  padding: 10px;
}

.card-title {
  margin-top: 10px;
  color: white;
  /* Cambia el color del texto del título de la película */
}

.pagination {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.pagination button {
  margin: 0 5px;
  padding: 5px 10px;
  cursor: pointer;
}

.pagination button[disabled] {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-primary {
  background-color: red;
  /* Cambia el color de fondo de los botones Anterior y Siguiente */
  color: white;
  /* Cambia el color del texto de los botones Anterior y Siguiente */
}

.current-page {
  margin: 0 10px;
  font-weight: bold;
  color: white;
  /* Cambia el color del número de página actual */
}

.loading-img {
  width: 100px;
  height: 150px;
  background-color: #222;
  animation: pulse 1.5s infinite;
}

@keyframes pulse {
  0% {
    opacity: 0.5;
  }

  50% {
    opacity: 1;
  }

  100% {
    opacity: 0.5;
  }
}
</style>