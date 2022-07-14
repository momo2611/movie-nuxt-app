<template>
  <div class="profile">
    <Modal
      v-if="modalActive"
      :modalMessage="modalMessage"
      @close-modal="closeModal"
    />
    <NuxtLink class="button" :to="{ name: 'index' }">Back</NuxtLink>

    <div class="container">
      <h2>Account Settings</h2>
      <div class="profile-info">
        <div class="initials"></div>
        <!-- email -->
        <div class="input">
          <label for="email">Email:</label>
          <input disabled type="text" id="email" placeholder="Email" />
        </div>
        <!-- new-password -->
        <div class="input">
          <label for="newpassword">New password:</label>
          <input
            placeholder="Enter your new password"
            type="password"
            id="newpassword"
            v-model="newpassword"
          />
        </div>
        <button @click.prevent="updateProfile">Save Changes</button>
      </div>
    </div>
  </div>
</template>

<script>
import { updatePassword } from 'firebase/auth'
import Modal from '../../components/Modal.vue'
export default {
  name: 'Profile',
  data() {
    return {
      cpassword: '',
      newpassword: '',
      modalMessage: 'Changes were saved!',
      modalActive: null,
    }
  },
  methods: {
    updateProfile() {
      const that = this
      updatePassword(this.$fire.auth.currentUser, this.newpassword)
        .then(() => {
          that.modalActive = !that.modalActive

          setTimeout(function () {
            window.location.reload(true)
          }, 10000)
        })
        .catch((err) => {
          that.modalActive = !that.modalActive
          that.modalMessage = err.message
        })
    },
    closeModal() {
      this.modalActive = !this.modalActive
    },
  },
  components: { Modal },
}
</script>
<style lang="scss" scoped>
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
  align-self: center;
  &:focus {
    outline: none;
  }
  &:hover {
    background-color: rgba(162, 123, 92, 0.7);
  }
}
.profile {
  background-color: #ccc;
  height: 100vh;
  .container {
    max-width: 1000px;
    padding: 60px 25px;

    h2 {
      text-align: center;
      margin-bottom: 16px;
      font-weight: 300;
      font-size: 32px;
    }

    .profile-info {
      border-radius: 8px;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
        0 2px 4px -1px rgba(0, 0, 0, 0.06);
      padding: 32px;
      background-color: #f1f1f1;
      display: flex;
      flex-direction: column;
      max-width: 600px;
      margin: 32px auto;

      .initials {
        position: initial;
        width: 80px;
        height: 80px;
        font-size: 32px;
        background-color: #303030;
        color: #fff;
        display: flex;
        align-self: center;
        align-items: center;
        justify-content: center;
        border-radius: 50%;
      }

      .input {
        margin: 16px 0;

        label {
          font-size: 16px;
          display: block;
          padding-bottom: 6px;
        }
        input {
          width: 100%;
          border: none;
          background-color: #f2f7f6;
          padding: 4px 4px 4px 24px;
          height: 46px;
          border-radius: 8px;

          &:focus {
            outline: none;
          }
        }
      }
    }
  }
}
</style>