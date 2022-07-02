<template>
  <div class="container">
    <TheLoading v-if="$fetchState.pending" />
    <div v-else class="movie">
      <div class="back">
        <nuxt-link to="/">Back</nuxt-link>
      </div>
      <div class="row">
        <div class="col-6">
          <img
            :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
            alt=""
            class="pic"
          />
        </div>
        <div class="col-6">
          <h1 class="title">{{ this.movie.title }}</h1>
          <h4 class="sub">{{ this.movie.tagline }}</h4>
          <p class="movie-fact">
            <span class="under">Released:</span>
            {{
              new Date(movie.release_date).toLocaleString('en-us', {
                month: 'long',
                day: 'numeric',
                year: 'numeric',
              })
            }}
          </p>
          <p class="duration">
            <span class="under">Duration</span>{{ this.movie.runtime }}
            <span class="min">mins</span>
          </p>
          <p class="txt">
            <span class="under">Duration</span> {{ this.movie.overview }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import TheLoading from '../../components/TheLoading.vue'
export default {
  name: 'InnerPage',
  data() {
    return {
      movie: null,
    }
  },
  async fetch() {
    await this.getSingleMovie()
  },
  fetchDelay: 1000,
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=fc57c5f989e21306c73a93c5427caff0`
      )
      const result = await data
      this.movie = result.data
      console.log(this.movie)
    },
  },
  components: { TheLoading },
}
</script>

<style scoped>
.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.back {
  padding: 0 0 4rem 0;
}
.back a {
  background: var(--red);
  color: white;
  padding: 0.5rem 4rem;
  font-size: 20px;
}
.pic {
  width: 100%;
  height: 700px;
  object-fit: cover;
}
.title {
  font-size: 48px;
  font-family: var(--decor);
  margin-bottom: 1rem;
}
.sub {
  font-size: 18px;
  font-style: italic;
  margin-bottom: 1rem;
}
.movie-fact,
.duration {
  font-size: 18px;
  margin-bottom: 1rem;
}
.txt {
  margin-top: 4rem;
  font-size: 20px;
  line-height: 150%;
}
</style>
