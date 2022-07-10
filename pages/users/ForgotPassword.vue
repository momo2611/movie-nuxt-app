<template>
  <div class="reset-password">
    <Modal
      v-if="modalActive"
      :modalMessage="modalMessage"
      @close-modal="closeModal"
    />
    <div class="form-wrap">
      <form class="reset">
        <p class="login-register">
          Back to
          <NuxtLink class="router-link" :to="{ name: 'users-Login' }"
            >Log in</NuxtLink
          >
        </p>
        <h2>Reset Password</h2>
        <p>Forgot your password? Enter your email to reset it</p>
        <div class="inputs">
          <div class="input">
            <input type="text" placeholder="Email" v-model="auth.email" />
            <img
              src="../../assets/Icons/envelope-regular.svg"
              alt=""
              class="icon"
            />
          </div>
        </div>
        <button @click.prevent="forgotPassword">Reset</button>
        <div class="angle"></div>
      </form>
      <div class="background"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      modalActive: false,
      modalMessage: '',
      auth: {
        email: '',
      },
    }
  },
  methods: {
    forgotPassword() {
      const that = this
      this.$fire.auth
        .sendPasswordResetEmail(this.auth.email)
        .then(() => {
          that.modalActive = true
          that.modalMessage =
            'If your account exists, you will receive a email to ' +
            that.auth.email
        })
        .catch(() => {
          that.modalActive = true
          that.modalMessage = 'User not found or the user may have been deleted'
        })
    },
    closeModal() {
      this.modalActive = !this.modalActive
      this.auth.email = ''
    },
  },
}
</script>

<style lang="scss" scoped>
.reset-password {
  position: relative;
  background-color: #2c3639;

  .form-wrap {
    .reset {
      h2 {
        margin-bottom: 8px;
      }
      p {
        text-align: center;
        margin-bottom: 32px;
        color: #dcd7c9;
      }
    }
  }
}
button {
  transition: 500ms ease all;
  cursor: pointer;
  margin-top: 24px;
  padding: 16px 32px;
  background-color: #3f4e4f;
  color: #fff;
  border-radius: 24px;
  border: none;
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
      font-size: 18px;
      margin: 16px 0 32px;
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