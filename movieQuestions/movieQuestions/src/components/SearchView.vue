<template>
    <div>
        <h1>What Movies did this Actor star in?</h1>
        <input v-model="actorName" placeholder="Enter Actor's Name" />
        <button @click="fetchMovies">Search</button>

        <ul v-if="movies.length">
            <li v-for="(movie, index) in movies" v-bind:key="index">{{  movie }}</li>
        </ul>
        <p v-if="error">{{ error }}</p>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            actorName: '',
            movies: [],
            error: '',
        };
    },
    methods: {
        async fetchMovies() {
            this.movies = [];
            this.error = '';

            const apiKey = '60fd73cd412866c2a1036d3546fb92a4';
            const actorSearchUrl = `https://api.themoviedb.org/3/search/person?api_key=${apiKey}&query=${this.actorName}`;

            try {


                const searchResponse = await axios.get(actorSearchUrl);
                const actor = searchResponse.data.results[0];

                if(actor) {
                    const movieCreditsUrl = `https://api.themoviedb.org/3/person/${actor.id}/movie_credits?api_key=${apiKey}`;
                    const creditsResponse = await axios.get(movieCreditsUrl);
                    this.movies = creditsResponse.data.cast.map(movie => movie.title);
                } else {
                    this.error = 'Actor not found.'
                }
            } catch (err) {
            this.error = 'An error occurred while fetching the data.';
            console.error(err);
            }
        }              
    }
}
</script>

<style>

li {
    list-style: none
}

</style>