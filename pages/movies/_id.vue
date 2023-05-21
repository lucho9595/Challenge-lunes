<template>
    <div class="all">
        <div class="container">
            <div class="row mt-3">
                <div class="col-12">
                    <router-link to="/movie" class="btn btn-primary">
                        <i class="fas fa-arrow-left"></i> Volver a películas
                    </router-link>
                </div>
            </div>
            <div class="row mt-3" v-if="movie">
                <div class="col-lg-4">
                    <img :src="movie.Poster" alt="Movie Poster" class="img-fluid mb-4">
                </div>
                <div class="col-lg-8">
                    <div class="movie-details">
                        <h1 class="mb-4">{{ movie.Title }}</h1>
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
            <div v-else class="loading text-center mt-3">Cargando detalles de la película...</div>
        </div>
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

<style scoped>
.all {
    display: flex;
    background-color: rgb(255 148 0 / 53%);
    min-height: 100vh;
    justify-content: center;
    align-items: center;
}

.container {
    display: flex;
    justify-content: center;
    background-color: rgba(35, 36, 36, 0.25098);
    border-radius: 15px;
    padding: 20px;
    max-width: 1200px;
    width: 100%;
    margin: 0 auto;
    flex-wrap: wrap;
    flex-direction: column;
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
    width: 100%;
    max-width: 300px;
    height: auto;
}

.details {
    text-align: left;
    padding-left: 30px;
    margin-top: 20px;
}

.loading {
    text-align: center;
    margin-top: 20px;
    font-style: italic;
}
</style>
