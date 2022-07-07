<template>
  <Loading v-if="$fetchState.pending" />
  <div v-else class="container single-movie">
    <NuxtLink class="button" :to="{ name: 'index' }">Back</NuxtLink>
    <div class="movie-trailer">
      <iframe
        width="560"
        height="315"
        :src="`https://www.youtube.com/embed/${movid.key}`"
        frameborder="0"
        gesture="media"
        allow="encrypted-media"
        allowfullscreen
      ></iframe>
    </div>
    <div class="lines">
      <div class="diamond"></div>
    </div>
    <div class="movie-info">
      <div class="movie-img">
        <img
          :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
          alt=""
        />
      </div>
      <div class="movie-content">
        <h1>{{ movie.title }}</h1>
        <p class="movie-fact tagline">
          <span>Tagline: </span> "{{ movie.tagline }}"
        </p>
        <p class="movie-fact">
          <span>Released:</span>
          {{
            new Date(movie.release_date).toLocaleString('en-us', {
              month: 'long',
              day: 'numeric',
              year: 'numeric',
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Duration:</span> {{ movie.runtime }} minutes
        </p>
        <p class="movie-fact">
          <span>Revenue:</span>
          {{
            movie.revenue.toLocaleString('en-us', {
              style: 'currency',
              currency: 'USD',
            })
          }}
        </p>
        <p class="movie-fact"><span>Overview:</span> {{ movie.overview }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Loading from '../../components/Loading.vue'
export default {
  name: 'single-movie',
  head() {
    return {
      title: this.movie.title,
    }
  },
  data() {
    return {
      movie: '',
      movid: '',
    }
  },
  async fetch() {
    await this.getSingleMovie()
    await this.getSingleVideo()
  },
  fetchDelay: 1000,
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=ad3c81ae840db058ad6cbfb16df65c81&language=en-US`
      )
      const result = await data
      this.movie = result.data
    },
    async getSingleVideo() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}/videos?api_key=ad3c81ae840db058ad6cbfb16df65c81&language=en-US`
      )
      const result = await data
      this.movid = result.data.results[0]
    },
  },
  components: { Loading },
}
</script>

<style lang="scss" scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;

  .lines {
    height: 45px;
    width: 60%;
    position: relative;
    margin: 24px auto;

    &::after,
    &::before {
      content: '';
      position: absolute;
      margin: auto;
      height: 2px;
      background: rgba(0, 0, 0, 0.4);
      width: 45%;
      top: 45%;
    }
    &::after {
      left: 0;
    }
    &::before {
      right: 0;
    }
    .diamond {
      height: 40px;
      width: 40px;
      border: 2px solid rgba(0, 0, 0, 0.4);
      left: 0;
      right: 0;
      top: 0;
      bottom: 0;
      margin: auto;
      transform: rotate(45deg);
    }
  }
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .movie-trailer {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 8px 0;
    height: 70vh;

    iframe {
      position: relative;
      top: 0;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 100%;
      border: 0;
    }
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;
        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
        margin-bottom: 36px;
      }
      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>