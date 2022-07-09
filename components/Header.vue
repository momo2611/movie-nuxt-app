<template>
  <div class="nav" id="nav">
    <img src="../assets/imgs/logoipsum-logo-40.svg" alt="" class="nav__logo" />
    <NuxtLink class="button button-light" :to="{ name: 'users-Register' }"
      >Login</NuxtLink
    >
  </div>
</template>

<script>
export default {
  methods: {
    handleScroll() {
      // Your scroll handling here
      if (window.scrollY)
        document.getElementById('nav').classList.add('nav__black')
      else document.getElementById('nav').classList.remove('nav__black')
    },
  },
  beforeMount() {
    window.addEventListener('scroll', this.handleScroll)
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.handleScroll)
  },
}
</script>

<style lang="scss" scoped>
.nav {
  position: fixed;
  top: 0;
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding: 20px;
  z-index: 99;
  transition: all 0.5s ease-in;
  .nav__logo {
    width: 160px;
    object-fit: contain;
  }
}
.nav__black {
  background: rgb(39, 36, 36);
  background: linear-gradient(
    180deg,
    rgba(39, 36, 36, 0.7458333675266982) 65%,
    rgba(242, 242, 242, 0) 100%
  );
}
</style>