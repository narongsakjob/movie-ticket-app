<template>
  <div class="box">
    <h3 class="title">App: {{movieId}}</h3>
    <movie @chooseMovie="handleChooseMovie" :movieId="movieId"></movie>
    <seat :movieId="movieId" 
          @chooseSeat="handleChooseSeat" 
          :selectSeats="selectSeats"
    />
  </div>
</template>
<script>
import Movie from 'Components/Movie.vue'
import Seat from 'Components/Seat.vue'

export default {
  components: { Movie, Seat },
  data() {
    return {
      movieId: '',
      selectSeats: []
    }
  },
  methods: {
    handleChooseMovie(movieId) {
      this.movieId = movieId
    },
    handleChooseSeat(seat) {
      const ids = this.selectSeats.map(s => s.id)
      const idx = ids.indexOf(seat.id)
      if(idx === -1) {
        this.selectSeats.push(seat)
      }else {
        this.selectSeats.splice(idx,1)
      }
    }
  }
}

</script>