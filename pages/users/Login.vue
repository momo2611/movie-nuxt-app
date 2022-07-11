<template>
  <div class="form-wrap">
    <form class="login">
      <p class="login-register">
        Don't have an account?
        <NuxtLink class="router-link" :to="{ name: 'users-Register' }"
          >Register</NuxtLink
        >
      </p>
      <h2>Login to MomoCmax</h2>
      <div class="inputs">
        <div class="input">
          <input type="text" placeholder="Email" v-model="auth.email" />
          <img
            src="../../assets/Icons/envelope-regular.svg"
            alt=""
            class="icon"
          />
        </div>
        <div class="input">
          <input
            type="password"
            placeholder="Password"
            v-model="auth.password"
          />
          <img
            src="../../assets/Icons/lock-alt-solid.svg"
            alt=""
            class="icon"
          />
        </div>
        <div v-show="error" class="error">{{ this.errorMsg }}</div>
      </div>
      <NuxtLink class="forgot-password" :to="{ name: 'users-ForgotPassword' }"
        >Forgot Your Password?</NuxtLink
      >
      <button @click.prevent="signIn">Sign In</button>
      <button @click.prevent="GoogleIn" class="btn-log">
        <img src="../../assets/imgs/google.PNG" alt="" class="icon" />
        <span>Log in with Google</span>
      </button>
      <div class="angle"></div>
    </form>
    <div class="background"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      error: false,
      errorMsg: '',
      auth: {
        email: '',
        password: '',
      },
    }
  },
  methods: {
    signIn() {
      const that = this
      this.$fire.auth
        .signInWithEmailAndPassword(this.auth.email, this.auth.password)

        .then((user) => {
          that.$router.push({ name: 'index' })
          that.error = false
          that.errorMsg = ''
        })
        .catch(function (err) {
          that.error = true
          that.errorMsg = err.message
        })
    },
    GoogleIn() {
      const that = this
      this.provider = new this.$fireModule.auth.GoogleAuthProvider()
      this.$fire.auth
        .signInWithPopup(this.provider)

        .then((user) => {
          that.$router.push({ name: 'index' })
          that.error = false
          that.errorMsg = ''
        })
        .catch(function (err) {
          that.error = true
          that.errorMsg = err.message
        })
    },
  },
}
</script>

<style lang="scss" scoped>
.btn-log {
  border-radius: 8px;
  margin-top: 32px;
  padding: 12px 24px;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  font-size: 16px;
  .icon {
    width: 18px;
    margin-right: 12px;
  }
}
.login {
  background-color: #2c3639;
}
button {
  transition: 500ms ease all;
  cursor: pointer;
  padding: 12px 28px;
  background-color: #3f4e4f;
  color: #fff;
  border-radius: 24px;
  border: none;
  font-size: 16px;
  text-transform: uppercase;
  &:focus {
    outline: none;
  }
  &:hover {
    background-color: rgba(162, 123, 92, 0.7);
  }
}
.error {
  text-align: center;
  font-size: 12px;
  color: red;
}
.form-wrap {
  overflow: hidden;
  display: flex;
  height: 100vh;
  justify-content: center;
  align-self: center;
  margin: 0 auto;
  width: 90%;
  @media (min-width: 900px) {
    width: 100%;
  }
  .login-register {
    margin-bottom: 32px;
    color: #dcd7c9;
    font-size: 20px;

    .router-link {
      color: #dcd7c9;
      &:hover {
        text-decoration: underline;
      }
    }
  }
  form {
    padding: 0 10px;
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    flex: 1;
    @media (min-width: 900px) {
      padding: 0 50px;
    }
    h2 {
      text-align: center;
      font-size: 32px;
      color: #dcd7c9;
      margin-bottom: 40px;
      @media (min-width: 900px) {
        font-size: 52px;
      }
    }
    .inputs {
      width: 100%;
      max-width: 362px;
      .input {
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        margin-bottom: 12px;
        input {
          width: 100%;
          border: none;
          background-color: #f2f7f6;
          padding: 4px 4px 4px 30px;
          height: 54px;
          border-radius: 8px;
          &:focus {
            outline: none;
          }
        }
        .icon {
          width: 12px;
          position: absolute;
          left: 6px;
        }
      }
    }
    .forgot-password {
      text-decoration: none;
      color: #dcd7c9;
      cursor: pointer;
      font-size: 14px;
      margin: 12px 0 20px;
      border-bottom: 1px solid transparent;
      transition: 0.5s ease all;
      &:hover {
        border-color: #dcd7c9;
      }
    }
    .angle {
      display: none;
      position: absolute;
      background-color: #2c3639;
      transform: rotateZ(3deg);
      width: 60px;
      right: -30px;
      height: 101%;
      @media (min-width: 900px) {
        display: initial;
      }
    }
  }
  .background {
    display: none;
    flex: 2;
    background-size: cover;
    background-image: url('../../assets/imgs/3883610.jpg');
    width: 100%;
    height: 100%;
    @media (min-width: 900px) {
      display: initial;
    }
  }
}
</style>