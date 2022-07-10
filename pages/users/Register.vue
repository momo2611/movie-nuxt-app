<template>
  <div class="form-wrap">
    <form class="register">
      <p class="login-register">
        Already have an account?
        <NuxtLink class="router-link" :to="{ name: 'users-Login' }"
          >Login</NuxtLink
        >
      </p>
      <h2>Create Your Account</h2>
      <div class="inputs">
        <div class="input">
          <input type="text" placeholder="First Name" v-model="firstName" />
          <img
            src="../../assets/Icons/user-alt-light.svg"
            alt=""
            class="icon"
          />
        </div>
        <div class="input">
          <input type="text" placeholder="Last Name" v-model="lastName" />
          <img
            src="../../assets/Icons/user-alt-light.svg"
            alt=""
            class="icon"
          />
        </div>
        <div class="input">
          <input type="text" placeholder="Username" v-model="username" />
          <img
            src="../../assets/Icons/user-alt-light.svg"
            alt=""
            class="icon"
          />
        </div>
        <div class="input">
          <input type="text" placeholder="Email" v-model="email" />
          <img
            src="../../assets/Icons/envelope-regular.svg"
            alt=""
            class="icon"
          />
        </div>
        <div class="input">
          <input type="password" placeholder="Password" v-model="password" />
          <img
            src="../../assets/Icons/lock-alt-solid.svg"
            alt=""
            class="icon"
          />
        </div>
        <div v-show="error" class="error"></div>
      </div>
      <button @click.prevent="register">Sign Up</button>
      <div class="angle"></div>
    </form>
    <div class="background"></div>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
.register {
  background-color: #2c3639;
  h2 {
    max-width: 350px;
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
    background-image: url('../../assets/imgs/bh-log.webp');
    width: 100%;
    height: 100%;
    @media (min-width: 900px) {
      display: initial;
    }
  }
}
</style>