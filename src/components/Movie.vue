<template>
    <div class="box">
        <h3 class="title">{{ movieId }}</h3>
        <div class="columns">
            <div v-for ="m in movies" class="column">
                <figure :class="className(m.id)" @click="chooseMovie(m.id)" >
                    <img :src="imgSrc(m.id)">
                </figure>
            </div>
        </div>
    </div>
</template>

<script>
    import { movies } from 'Others/movie.json'
    export default {
        props: [ 'movieId' ],
        data() {
            return {
                movies
            }
        },
        methods: {
            imgSrc(movieId) {
                return `/movies/${ movieId }.jpg`
            },
            chooseMovie(movieId) {
                this.$emit('chooseMovie',movieId)
            },
            className(movieId) {
                return [
                    'image', 'pointer', { 'chosen': this.movieId == movieId}
                ]
            }
        },
        mounted() {
            this.chooseMovie(movies[0].id);
        }
    }
</script>

<style>
    .pointer {
        cursor: pointer;
    }
    .chosen {
        border: 5px solid red;
    }
</style>