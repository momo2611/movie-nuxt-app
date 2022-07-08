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
    <!-- Movie info -->
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
    <h3 class="recom-text">Recommendation</h3>
    <!-- Recommend -->
    <div class="movies">
      <div id="movie-grid" class="movies-grid">
        <div
          v-for="(moviem, index) in recomMovies.slice(0, 4)"
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
      recomMovies: [],
    }
  },
  async fetch() {
    await this.getSingleMovie()
    await this.getSingleVideo()
    await this.getRecommend()
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
    async getRecommend() {
      const data = axios.get(`
      https://api.themoviedb.org/3/movie/${this.$route.params.movieid}/recommendations?api_key=ad3c81ae840db058ad6cbfb16df65c81&language=en-US&page=1`)
      const result = await data

      console.log(result.data)
      result.data.results.forEach((movie) => {
        this.recomMovies.push(movie)
      })
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
  .recom-text {
    margin-top: 28px;
    font-size: 42px;
    font-weight: 500;
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
}
</style>