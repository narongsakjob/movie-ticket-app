<template>
  <div class="box">
    <h3 class="title">App: {{movieId}}</h3>
    <p> Count: {{status.count}} </p>
    <p> Price: {{status.price}} </p>
    <movie @chooseMovie="handleChooseMovie" :movieId="movieId"></movie>
    <seat :movieId="movieId" 
          @chooseSeat="handleChooseSeat" 
          :selectSeats="selectSeats"
          :firebaseSeats="firebaseSeats"
    />
  </div>
</template>
<script>
import firebase from 'firebase'
import Movie from 'Components/Movie.vue'
import Seat from 'Components/Seat.vue'
import { pushToArray } from 'Others/lib'
import _ from 'lodash'

const config = {
  databaseURL: "https://realtimedatabase-cb77e.firebaseio.com",
}
firebase.initializeApp(config)

const db = firebase.database()

export default {
  components: { Movie, Seat },
  data() {
    return {
      movieId: '',
      selectSeats: [],
      firebaseSeats: [],
      status: { count: 0, price: 0 }
    }
  },
  methods: {
    handleChooseMovie(movieId) {
      if(this.status.count) {
        if( confirm("Data Will be lost ?") ) {
            this.status = { count: 0, price: 0 }
            this.selectSeats = []
        }else {
          return false
        }
      }
      this.movieId = movieId

      const movieRef = db.ref('/').child(this.movieId)
      movieRef.on('value', snapshot => {
        const seats = snapshot.val()
        this.firebaseSeats = []

        _.forOwn(seats, s => {
          pushToArray(s, this.firebaseSeats)
        })
      })

    },
    handleChooseSeat(seat) {

      pushToArray(seat, this.selectSeats)
      const movieRef = db.ref('/').child(this.movieId)
      movieRef.push(seat)

      this.status = this.selectSeats.reduce((summary, s) => {
        summary.count++
        summary.price += s.price
        return summary
      }, { count: 0, price: 0 })
    }
  }
}

</script>