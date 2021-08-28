<template>
<div>
    <div>
       <h4>Sign in</h4>
    </div>
   <form>
  <div class="form-group">
    <label for="exampleFormControlInput1">Email</label>
    <input type="email" class="form-control" id="exampleFormControlInput1" placeholder="Email" v-model="loginInfo.email">
  </div>
  <div class="form-group">
    <label for="exampleFormControlInput2">Password</label>
    <input type="password" class="form-control" id="exampleFormControlInput2" placeholder="Password" v-model="loginInfo.password">
  </div>
  <div class="form-group">
    <a href="#">Forgot password</a>
  </div>
  <div class="form-group">
    <button type="button" class="btn btn-info btn-lg btn-block" v-on:click.prevent="loginCustom">Login</button>
  </div>
  <div class="form-group">
    <button type="button" class="btn btn-secondary btn-lg btn-block">I don't have an account</button>
  </div>
</form>
</div>
</template>

<script>
import axios from 'axios'
axios.defaults.headers.get['Content-Type'] = 'application/x-www-form-urlencoded'
axios.defaults.headers.post['Content-Type'] = 'multipart/form-data'
import { postRequest } from '@/utils/http';
import forms from '@/mixins/forms';

export default {
  name:"CustomLogIn",

  data(){
    return {
      loginInfo: {
          email: '',
          password: '',
          token: localStorage.getItem('user-token') || null,
        }
    }
  },

  mixins: [ forms ],

  // @todo please use one empty line to separate elements like data, mixins, methods, mounted...
  methods:{
    // @todo my version
    login() {
      const data = { 
        username: this.loginInfo.email, 
        password: this.loginInfo.password 
      };

      return new Promise((resolve, reject) => {
        postRequest('/login', data)
          .then((response) => {
            // @todo this code is from my another project, please use as reference only
            this.$store.dispatch('app/login');
            this.$store.dispatch('app/setUser', response.user);
            const intended = window.sessionStorage.getItem('intended');
            if (intended) {
              this.$router.push(intended);
              window.sessionStorage.removeItem('intended');
            } else if (response.user.role === 'admin') {
              this.$router.push({ name: 'admin.dashboard' });
            } else {
              this.$router.push({ name: 'user.dashboard' });
            }

            resolve(response);
          })
          .catch((error) => {
            reject(error);
          });
      });
    },

      loginCustom(){
        const data = { username: this.email, password: this.password };
        axios
            .post('https://static.motivo.localhost/api/token/', data)
            .then(resp=>{
              this.token=resp.data.access
              console.log(resp.data)
              //localStorage.setItem('user-token', resp.data.access)
            })
            .catch(error => console.log(error))
            
      }
      
  },
}

</script>

<style scoped>
 form {
   width: 50%;
   margin: auto;
 }
 h4 {
   text-align: center;
 }
 a {
   color: #1CB0F6;
   font-weight: bold;
 }

 input, button {
   border-radius: 12px;
 }
</style>