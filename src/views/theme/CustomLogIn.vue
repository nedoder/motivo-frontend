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
  methods:{
      loginCustom(){
        console.log("lala");
        axios
            .post('https://static.motivo.localhost/api/token/',{
                  username: this.email,
                  password: this.password,
            })
            .then(resp=>{
              this.token=resp.data.access
              console.log(resp.data)
              localStorage.setItem('user-token', resp.data.access)
            })
            .catch(error => console.log(error))
            
      }
      
  },
}

</script>