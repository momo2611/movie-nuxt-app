<template>
  <div class="hero">
    <img
      :src="`https://image.tmdb.org/t/p/original/${movies1.backdrop_path}`"
      alt=""
    />
    <div class="text-container">
      <div class="text">
        <h1>{{ movies1.original_title }}</h1>
        <div class="text-overview">{{ movies1.overview }}</div>
        <a href="#movie-grid" class="button">Explore</a>
      </div>
    </div>
    <div class="banner--fadeBottom"></div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Hero',
  data() {
    return {
      movies1: [],
    }
  },
  async fetch() {
    await this.popularMovie()
  },
  fetchDelay: 1000,
  methods: {
    async popularMovie() {
      const data = axios.get(
        'https://api.themoviedb.org/3/movie/popular?api_key=ad3c81ae840db058ad6cbfb16df65c81&language=en-US&page=1'
      )
      const result = await data
      this.movies1 = result.data.results[0]
    },
  },
}
</script>

<style lang="scss" scoped>
.hero {
  height: 400px;
  position: relative;
  @media (min-width: 750px) {
    height: 564px;
  }
  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-image: linear-gradient(
      180deg,
      rgba(0, 0, 0, 0.5),
      rgba(0, 0, 0, 0.3),
      transparent
    );
  }
  &::before {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 7.8rem;
    background-image: linear-gradient(
      180deg,
      transparent,
      rgba(37, 37, 37, 0.61),
      #111
    );
  }
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .text-container {
    z-index: 98;
    position: absolute;
    top: 0;
    margin: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    .text {
      padding: 0 16px;
      width: 100%;
      max-width: 1400px;
      margin: 0 auto;
    }
    .text-overview {
      color: #fff;
      line-height: 1.3;
      padding-top: 1rem;
      font-size: 0.8rem;
      max-width: 60%;
      display: -webkit-box;
      -webkit-line-clamp: 3;
      -webkit-box-orient: vertical;
      overflow: hidden;
      margin-bottom: 12px;
      @media (min-width: 750px) {
        font-size: 22px;
      }
    }
    h1 {
      color: #fff;
      font-size: 42px;
      font-weight: 300;
      margin-bottom: 8px;
      @media (min-width: 750px) {
        font-size: 72px;
      }
    }
    .button {
      font-size: 20px;
      align-self: flex-start;
    }
  }
}
</style>