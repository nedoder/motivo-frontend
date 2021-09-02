<template>
  <div class="wrapper-in">
    <div class="motivo">
      <img src="./img/motivo.png" />
      <div class="formcontainer">
        <form>
          <div>
            <h4>Sign in</h4>
          </div>
          <div class="form-group">
            <label for="exampleFormControlInput1">Email</label>
            <input type="email" class="form-control" id="exampleFormControlInput1" placeholder="Email"
              v-model="loginInfo.email">
          </div>
          <div class="form-group">
            <label for="exampleFormControlInput2">Password</label>
            <input type="password" class="form-control" id="exampleFormControlInput2" placeholder="Password"
              v-model="loginInfo.password">
          </div>
          <div class="form-group">
            <a href="#">Forgot password</a>
          </div>
          <div class="form-group">
            <button type="button" class="btn btn-info btn-lg btn-block" v-on:click.prevent="loginCustom">Login</button>
          </div>
          <div class="form-group">
            <button type="button" class="btn btn-link btn-lg btn-block">I don't have an account</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
  axios.defaults.headers.get['Content-Type'] = 'application/x-www-form-urlencoded'
  axios.defaults.headers.post['Content-Type'] = 'multipart/form-data'
  import {
    postRequest
  } from '@/utils/http';
  import forms from '@/mixins/forms';

  export default {
    name: "CustomLogIn",
    data() {
      return {
        loginInfo: {
          email: '',
          password: '',
          token: localStorage.getItem('user-token') || null,
        }
      }
    },

    mixins: [forms],

    // @todo please use one empty line to separate elements like data, mixins, methods, mounted...
    methods: {
      // @todo my version
      login() {
        const data = {
          username: this.loginInfo.email,
          password: this.loginInfo.password
        };
      },

      loginCustom() {
        const data = {
          username: this.loginInfo.email,
          password: this.loginInfo.password
        };
        axios({
            method: 'post',
            url: '/api/token/',
            data: data,
          })
          .then(resp => {
            this.token = resp.data.access
            console.log(resp.data)
            localStorage.setItem('user-token', resp.data.access)
            this.$router.push('/onboarding1')
          })
          .catch(error => console.log(error))
      },
    }
  }
</script>

<style scoped>
  .wrapper-in {
    background: #ffffff;
    height: 100vh;
  }

  form {
    width: 100%;
  }

  .formcontainer {
    width: 40%;
    margin: auto;
    margin-top: 10%;
  }

  .motivo {
    width: 100%;
    margin: auto;
    padding: 30px 50px;
  }

  h4 {
    text-align: center;
    padding-bottom: 1rem;
    font-size: 24px;
    color: #6D7885;
  }

  a,
  .btn-link {
    color: #1CB0F6;
    font-weight: bold;
    font-size: 18px;
  }

  label {
    color: #99A2AD;
    font-size: 14px;
  }

  .btn-link {
    box-shadow: 1px 1px 3px #99A2AD;
    text-decoration: none;
  }

  .btn-info {
    font-weight: bold;
  }

  input,
  button {
    border-radius: 12px;
    padding: 10px;
  }

  input {
    font-size: 18px;
    background: #F7F8FA;
  }
</style>