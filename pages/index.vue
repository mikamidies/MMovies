<template>
  <div>
    <HomeHeader />

    <div class="container">
      <div class="search">
        <input
          v-model.lazy="searchInput"
          class="search-input"
          type="text"
          placeholder="search"
          @keyup.enter="$fetch"
        />
        <button
          @click="clearSearch"
          v-show="searchInput !== ''"
          class="clear-btn"
        >
          Clear
        </button>
      </div>
    </div>

    <TheLoading class="the-loading" v-if="$fetchState.pending" />

    <div v-else class="container">
      <div v-if="searchInput !== ''" class="movie-grid" id="grid">
        <div
          v-for="(movie, index) in searchedMovies"
          :key="index"
          class="movie"
        >
          <nuxt-link
            :to="{ name: 'movie-movieid', params: { movieid: movie.id } }"
          >
            <div class="movie-img">
              <img
                :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                alt=""
              />
              <p class="review">{{ movie.vote_average }}</p>
              <div class="overview">
                <p>{{ movie.overview }}</p>
              </div>
            </div>
            <div class="info">
              <p class="movie-title">
                {{ movie.title.slice(0, 25) }}
                <span v-if="movie.title.length > 25">...</span>
              </p>
              <p class="release">
                Released:
                {{
                  new Date(movie.release_date).toLocaleString('en-us', {
                    month: 'long',
                    day: 'numeric',
                    year: 'numeric',
                  })
                }}
              </p>
              <button class="btn">More info</button>
            </div>
          </nuxt-link>
        </div>
      </div>

      <div v-else class="movie-grid" id="grid">
        <div v-for="(movie, index) in movies" :key="index" class="movie">
          <nuxt-link
            :to="{ name: 'movie-movieid', params: { movieid: movie.id } }"
          >
            <div class="movie-img">
              <img
                :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                alt=""
              />
              <p class="review">{{ movie.vote_average }}</p>
              <div class="overview">
                <p>{{ movie.overview }}</p>
              </div>
            </div>
            <div class="info">
              <p class="movie-title">
                {{ movie.title.slice(0, 25) }}
                <span v-if="movie.title.length > 25">...</span>
              </p>
              <p class="release">
                Released:
                {{
                  new Date(movie.release_date).toLocaleString('en-us', {
                    month: 'long',
                    day: 'numeric',
                    year: 'numeric',
                  })
                }}
              </p>
              <button class="btn">More info</button>
            </div>
          </nuxt-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import TheLoading from '../components/TheLoading.vue'

import HomeHeader from '~/components/HomeHeader.vue'

export default {
  name: 'IndexPage',
  components: { HomeHeader, TheLoading },

  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchInput: '',
    }
  },

  async fetch() {
    if (this.searchInput === '') {
      await this.getMovies()
    }
    if (this.searchInput !== '') {
      await this.searchMovies()
    }
  },
  fetchDelay: 1000,

  methods: {
    async getMovies() {
      const data = axios.get(
        'https://api.themoviedb.org/3/movie/now_playing?api_key=fc57c5f989e21306c73a93c5427caff0'
      )
      const result = await data
      result.data.results.forEach((movie) => {
        this.movies.push(movie)
      })
    },
    async searchMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=fc57c5f989e21306c73a93c5427caff0&page=1&query=${this.searchInput}`
      )
      const result = await data
      result.data.results.forEach((movie) => {
        this.searchedMovies.push(movie)
      })
    },

    clearSearch() {
      this.searchInput = ''
      this.searchedMovies = []
    },
  },
}
</script>

<style scoped>
.the-loading {
  padding-top: 120px;
  align-items: flex-start;
}
.movie-grid {
  padding: 60px 0 120px 0;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  row-gap: 56px;
}
.movie-img img {
  width: 100%;
  height: 500px;
  object-fit: cover;
}
.movie-img {
  position: relative;
  overflow: hidden;
  margin-bottom: 2rem;
}
.review {
  position: absolute;
  top: 0;
  left: 0;
  padding: 10px;
  background: var(--red);
  font-weight: 500;
  font-size: 18px;
}
.overview {
  position: absolute;
  bottom: 0;
  left: 0;
  /* height: 50%; */
  width: 100%;
  background: var(--red);
  display: flex;
  align-items: center;
  justify-content: center;
  /* text-align: center; */
  transition: 0.5s;
  transform: translateY(100%);
  padding: 1rem 1.5rem 0 1.5rem;
}
.overview p {
  font-size: 18px;
  font-weight: 400;
  line-height: 150%;
  display: -webkit-box;
  -webkit-line-clamp: 6;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
.movie:hover .overview {
  transform: translateY(0);
}
.movie-title {
  font-size: 24px;
  font-weight: 500;
}
.release {
  font-weight: 300;
  font-size: 14px;
  opacity: 0.8;
  margin-bottom: 2rem;
}
.btn {
  font-size: 20px;
  color: white;
  border: 1px solid var(--red);
  padding: 0.7rem 3rem;
  border-radius: 0;
  transition: 0.5s;
  position: relative;
  overflow: hidden;
}
.btn::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: var(--red);
  z-index: -1;
  transform: translateY(100%);
  transition: 0.5s;
}
.movie:hover .btn::after {
  transform: translateY(0);
}
.movie {
  cursor: pointer;
}
.movie a {
  color: white;
}
.search {
  padding: 2rem 0;
}
.search-input {
  padding: 1rem;
  border: none;
  border-radius: 0;
  font-size: 17px;
}
.clear-btn {
  padding: 15px;
  border: none;
  border-radius: 0;
  background: var(--red);
  color: white;
  font-weight: 500;
  font-size: 18px;
}
</style>
