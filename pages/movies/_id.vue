<template>
    <div class="all">
        <router-link to="/movie" class="back-button">
            <i class="fas fa-arrow-left"></i> Volver a películas
        </router-link>
        <div v-if="movie" class="container">
            <div class="movie-details">
                <h1 class="mb-4">{{ movie.Title }}</h1>
                <div class="row">
                    <div class="col-md-4">
                        <img :src="movie.Poster" alt="Movie Poster" class="img-fluid mb-4">
                    </div>
                    <div class="col-md-8">
                        <div class="details">
                            <p><strong>Año:</strong> {{ movie.Year }}</p>
                            <p><strong>Género:</strong> {{ movie.Genre }}</p>
                            <p><strong>Director:</strong> {{ movie.Director }}</p>
                            <p><strong>Actores:</strong> {{ movie.Actors }}</p>
                            <p><strong>Descripción:</strong> {{ movie.Plot }}</p>
                            <p><strong>Premios:</strong> {{ movie.Awards }}</p>
                            <div v-if="movie.Ratings.length > 0">
                                <h3>Ratings:</h3>
                                <ul class="list-unstyled">
                                    <li v-for="rating in movie.Ratings" :key="rating.Source">
                                        <strong>{{ rating.Source }}:</strong> {{ rating.Value }}
                                    </li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div v-else class="loading">Cargando detalles de la película...</div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: "detail",
    data() {
        return {
            movie: null
        };
    },
    mounted() {
        this.fetchMovieDetails();
    },
    methods: {
        async fetchMovieDetails() {
            try {
                const movieId = this.$route.params.id;
                const apiKey = '7e46d54c';
                const url = `https://www.omdbapi.com/?apikey=${apiKey}&i=${movieId}`;
                const response = await axios.get(url);
                if (response.data.Response === 'True') {
                    this.movie = response.data;
                } else {
                    console.error('Error al obtener los detalles de la película:', response.data.Error);
                }
            } catch (error) {
                console.error('Error al realizar la solicitud a la API de OMDB:', error);
            }
        }
    }
};
</script>

<style>
.back-button {
    position: absolute;
    top: 20px;
    left: 20px;
    color: white;
    font-size: 1.5rem;
    text-decoration: none;
    transition: opacity 0.3s ease;
}

.back-button:hover {
    opacity: 0.7;
}

.all {
    background-color: rgba(0, 0, 0, 0.849);
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;
}

.movie-details {
    display: flex;
    text-align: center;
    color: white;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    flex-wrap: wrap;
}

.movie-details img {
    width: 300px;
}

.details {
    text-align: left;
    padding-left: 30px;
}

.loading {
    text-align: center;
    margin-top: 20px;
    font-style: italic;
}
</style>
