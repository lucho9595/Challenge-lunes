<template>
    <div class="movie-container">
        <h1>Películas</h1>
        <div class="movie-grid">
            <div v-for="movie in displayedMovies" :key="movie.imdbID" class="movie-card" @click="goToDetail(movie)">
                <img :src="movie.Poster" alt="Movie Poster" class="movie-poster">
                <h2>{{ movie.Title }}</h2>
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
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            movies: [],
            currentPage: 1,
            moviesPerPage: 10
        };
    },
    mounted() {
        this.fetchMovies();
    },
    computed: {
        displayedMovies() {
            const startIndex = (this.currentPage - 1) * this.moviesPerPage;
            const endIndex = startIndex + this.moviesPerPage;
            return this.movies.slice(startIndex, endIndex);
        },
        totalPages() {
            return Math.ceil(this.movies.length / this.moviesPerPage);
        }
    },
    methods: {
        async fetchMovies() {
            try {
                const apiKey = '7e46d54c';
                const url = `https://www.omdbapi.com/?apikey=${apiKey}&s=movie&type=movie&page=${this.currentPage}`;
                const response = await axios.get(url);
                if (response.data.Response === 'True') {
                    this.movies = response.data.Search;
                } else {
                    console.error('Error al obtener la lista de películas:', response.data.Error);
                }
            } catch (error) {
                console.error('Error al realizar la solicitud a la API de OMDB:', error);
            }
        },
        goToDetail(movie) {
            this.$router.push(`/movies/${movie.imdbID}`);
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++;
                this.fetchMovies();
            }
        },
        previousPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
                this.fetchMovies();
            }
        }
    }
};
</script>
<style>
.movie-container {
    background-color: #000;
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
