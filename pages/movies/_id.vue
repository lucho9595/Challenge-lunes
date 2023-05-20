<template>
    <div v-if="movie" class="movie-details">
        <h1>{{ movie.Title }}</h1>
        <div class="details">
            <img :src="movie.Poster" alt="Movie Poster">
            <div>
                <p><strong>Año:</strong> {{ movie.Year }}</p>
                <p><strong>Género:</strong> {{ movie.Genre }}</p>
                <p><strong>Director:</strong> {{ movie.Director }}</p>
                <p><strong>Actores:</strong> {{ movie.Actors }}</p>
                <p><strong>Descripción:</strong> {{ movie.Plot }}</p>
            </div>
        </div>
    </div>
    <div v-else class="loading">Cargando detalles de la película...</div>
</template>

<script>
import axios from 'axios';

export default {
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
.movie-details {
    text-align: center;
}

.movie-details img {
    width: 300px;
    margin-bottom: 20px;
}

.details {
    display: flex;
    justify-content: center;
    align-items: flex-start;
}

.loading {
    text-align: center;
    margin-top: 20px;
    font-style: italic;
}
</style>
