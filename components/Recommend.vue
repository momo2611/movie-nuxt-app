<template>
  <div class="movies">
    <div id="movie-grid" class="movies-grid">
      <div
        v-for="(moviem, index) in recomMovies.slice(0, 8)"
        :key="index"
        class="movie"
      >
        <div class="movie-img">
          <img
            :src="`https://image.tmdb.org/t/p/w500/${moviem.poster_path}`"
            alt=""
          />
          <p class="review">{{ moviem.vote_average }}</p>
          <p class="overview">{{ moviem.overview }}</p>
        </div>
        <div class="info">
          <p class="title">
            {{ moviem.title.slice(0, 25) }}
            <span v-if="moviem.title.length > 25">...</span>
          </p>
          <p class="release">
            Release:
            {{
              new Date(moviem.release_date).toLocaleString('en-us', {
                month: 'long',
                day: 'numeric',
                year: 'numeric',
              })
            }}
          </p>
          <NuxtLink
            class="button button-light"
            :to="{ name: 'movies-movieid', params: { movieid: moviem.id } }"
            >Get More Info</NuxtLink
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      recomMovies: [],
    }
  },
  async fetch() {
    await this.getRecommend()
  },
  fetchDelay: 1000,
  methods: {
    async getRecommend() {
      const data = axios.get(`
      https://api.themoviedb.org/3/movie/${this.$route.params.movieid}/recommendations?api_key=ad3c81ae840db058ad6cbfb16df65c81&language=en-US&page=1`)
      const result = await data
      result.data.results.forEach((movie) => {
        this.recomMovies.push(movie)
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.movies {
  padding: 32px 0;
  .movies-grid {
    display: grid;
    column-gap: 32px;
    row-gap: 64px;
    grid-template-columns: 1fr;
    @media (min-width: 500px) {
      grid-template-columns: repeat(2, 1fr);
    }
    @media (min-width: 750px) {
      grid-template-columns: repeat(2, 1fr);
    }
    @media (min-width: 1100px) {
      grid-template-columns: repeat(4, 1fr);
    }
    .movie {
      position: relative;
      display: flex;
      flex-direction: column;
      .movie-img {
        position: relative;
        overflow: hidden;
        &:hover {
          .overview {
            transform: translateY(0);
          }
        }
        img {
          display: block;
          width: 100%;
          height: 100%;
        }
        .review {
          position: absolute;
          top: 0;
          left: 0;
          display: flex;
          justify-content: center;
          align-items: center;
          width: 40px;
          height: 40px;
          background-color: #c92502;
          color: #fff;
          border-radius: 0 0 16px 0;
          box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
            0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }
        .overview {
          line-height: 1.5;
          position: absolute;
          bottom: 0;
          background-color: rgba(201, 38, 2, 0.9);
          padding: 12px;
          color: #fff;
          transform: translateY(100%);
          transition: 0.3s ease-in-out all;
        }
      }
      .info {
        margin-top: auto;
        .title {
          margin-top: 8px;
          color: #fff;
          font-size: 20px;
        }
        .release {
          margin-top: 8px;
          color: #c9c9c9;
        }
        .button {
          margin-top: 8px;
        }
      }
    }
  }
}
</style>