<template>
<div>
  <div class="login-container">
    <div class="login-form">
      <div class="login-header">
        <div class="login-title"><span>Login</span></div>
      </div>

      <form>
        <input type="text" class="login-input" placeholder="Email" v-model="email" />
        <input type="password" class="login-input" placeholder="Password" v-model="password" />
        <button type="submit" class="login-button" v-on:click.prevent="login">Login</button>
      </form>
    </div>
  </div>
</div>
</template>


<script>
import router from '@/router'
import store from '@/store/store'
import service from '@/api/service'

export default {
  data() {
    return {
      email: 'username@mail.com',
      password: 'password'
    }
  },

  methods: {
    login() {
      service.login({
        email: this.email,
        password: this.password
      }).then(response => {
        this.loginCheck(response)
      }, error => {
        this.loginFailure(error) })
    },

    loginCheck(response) {
      let status = response.status
      console.log('status: ' + status)
      if (status === 200) {
        let token = response.data.token
        console.log('token: ' + token)
        let profile = {
          username: this.email
        }
        this.loginSuccess(token, profile)
      } else if (status === 'error') {
        this.loginFailure(response.data.errors)
      }
    },

    loginSuccess(token, profile) {
      console.log('access granted: ', token, profile)

      store.dispatch('login', {
        token: token,
        profile: profile
      }).then(() => {
        router.push('/')
      })
    },

    loginFailure(error) {
      console.err('cannot execute login: ', error)
      store.dispatch('logout')
    }
  }
}
</script>


<style scoped>
.login-container {
  position: fixed;
  left: 0px;
  top: 0px;
  width: 100%;
  height: 100%;
  z-index: 50;
  display: flex;
  justify-content: space-around;
  background-color: #7FC4F0;
  background-image: url(../assets/background/light.svg), radial-gradient(farthest-side ellipse at 10% 0, #7FC4F0 20%, #C1DDE6);
  background-image: url(../assets/background/light.svg), -webkit-radial-gradient(10% 0, farthest-side ellipse, #7FC4F0 20%, #C1DDE6);
  background-image: url(../assets/background/light.svg), -moz-radial-gradient(10% 0, farthest-side ellipse, #7FC4F0 20%, #C1DDE6);
  background-size: cover;
  background-attachment: fixed, fixed;
  color: #515151;
}

.login-form {
  position: relative;
  margin: auto;
  padding: 40px;
  height: 25%;
  text-align: center;
  background-color: rgba(255, 255, 255, 0.4);
  box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.15);
  border-radius: 4px;
}

.login-header {
  width: 100%;
  height: 50px;
  margin: 10px auto;
  font-weight: bold;
  font-size: 20px;
  color: #515151;
  display: flex;
  justify-content: space-around;
}

.login-title {
  display: flex;
  margin-left: 20px;
}

.login-title span {
  margin: auto;
}

.login-input {
  padding: 8px;
  width: 50%;
  margin-bottom: 20px;
  background-color: rgba(255, 255, 255, 0.4);
  border: none;
  border-radius: 4px;
}

.login-button {
  background-color: #515151;
  color: white;
  font-size: 16px;
  width: 50%;
  padding: 8px;
  border: none;
  cursor: pointer;
  border-radius: 4px;
}

.login-button:hover {
  opacity: 0.9;
}
</style>
