<<template>
  <div class="box">
    <h3 class="title">Movie :  {{ movieId }}</h3>
    <p>Count: {{status.count }} , Price: {{status.price}} </p>
    <Movie @chooseMovie="handleChooseMovie" :movieId="movieId" />
    <seat 
    @chooseSeat="handleChooseSeat" 
    :movieId="movieId" 
    :selectSeats="selectSeats"
    />
  </div>
  
  
  
  
</template>

<script>
  import Movie from './Movie.vue'
  import Seat from './Seat.vue'

  export default {
    components: { Movie , Seat },
    data() {
      return {
        movieId: '',
        selectSeats:[],
        status: {count: 0, price: 0}
      }
    },
    methods: {
      handleChooseMovie(movieId){
        if (this.status.count != 0){
          if (confirm('Data will be lost ?')){
            this.status = {count: 0 , price: 0}
            this.selectSeats = []
          }
          else {
            return false
          }
        }
        this.movieId = movieId
      },
      handleChooseSeat(seat){
        const ids = this.selectSeats.map(s => s.id)
        const idx = ids.indexOf(seat.id)
        if (idx === -1){
        this.selectSeats.push(seat)
        }
        else {
          this.selectSeats.splice(idx,1)
        }
        
        this.status = this.selectSeats.reduce((summary, s) => {
           summary.count++
           summary.price += s.price
           return summary
        }, { count:0,price:0})
      }
    }
  }
</script>
