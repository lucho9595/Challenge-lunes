<template>
    <div class="container-fluid">
        <div class="movie-container">
            <h1>Películas</h1>
            <div class="search-container">
                <input type="text" v-model="searchQuery" @input="searchMovies" placeholder="Buscar película"
                    class="search-input" />
                <button @click="clearSearch">Limpiar</button>
            </div>
            <div class="row">
                <div v-for="movie in movies" :key="movie.imdbID" class="col-12 col-md-6 col-lg-4 movie-card">
                    <img :src="movie.Poster" alt="Movie Poster" class="movie-poster" @click="goToDetail(movie)">
                    <h2>{{ movie.Title }}</h2>
                </div>
            </div>
            <div class="pagination">
                <button @click="previousPage" :disabled="currentPage === 1" class="btn btn-primary">
                    Anterior
                </button>
                <span class="current-page">{{ currentPage }}</span>
                <button @click="nextPage" class="btn btn-primary">
                    Siguiente
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: 'movie',
    data() {
        return {
            movies: [], // Lista de películas
            currentPage: 1, // Página actual
            totalResults: 0, // Total de resultados
            apiKey: '7e46d54c', // Reemplaza "tu-api-key" con tu propia clave de API
            searchQuery: '', // Consulta de búsqueda
        };
    },
    mounted() {
        this.fetchMovies();
    },
    methods: {
        fetchMovies() {
            const url = `https://www.omdbapi.com/?apikey=${this.apiKey}&s=movie&type=movie&page=${this.currentPage}`;
            axios
                .get(url)
                .then(response => {
                    this.movies = response.data.Search || [];
                    this.totalResults = parseInt(response.data.totalResults);
                })
                .catch(error => {
                    console.error('Error al obtener las películas:', error);
                });
        },
        nextPage() {
            if (this.currentPage < Math.ceil(this.totalResults / 10)) {
                this.currentPage++;
                this.fetchMovies();
                this.searchMovies(); // Agregamos la llamada a searchMovies para mantener la búsqueda activa en la nueva página
            }
        },
        previousPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
                this.fetchMovies();
                this.searchMovies(); // Agregamos la llamada a searchMovies para mantener la búsqueda activa en la nueva página
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
                return this.movies.filter((movie) =>
                    movie.Title.toLowerCase().includes(this.searchQuery.toLowerCase())
                );
            }
        },
    },
};
</script>

<style>
h1 {
    color: red;
    text-align: center;
    margin-bottom: 20px;

}

h1:hover {
    text-shadow: -3px 2px 50px white;
}

.container-fluid {
    background-color: black;
}

.search-container {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 20px;
}

.search-input {
    width: 200px;
    margin-right: 10px;
}

.movie-container {
    padding: 20px;
}

.movie-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 20px;
}

.movie-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
}

.movie-poster {
    width: 200px;
    cursor: pointer;
    transition: transform 0.3s ease;
}

.movie-poster:hover {
    transform: scale(1.1);
}

.movie-card h2 {
    margin-top: 10px;
    color: #fff;
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
    color: #fff;
    border: none;
    padding: 10px 20px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.btn-primary:hover {
    background-color: #c80000;
}
</style>
