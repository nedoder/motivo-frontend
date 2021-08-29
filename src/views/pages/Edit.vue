<template>
    <CWrapper>
    <TheHeader/>
<div class="formcontainer">
   <form>
     <div>
       <h4>Profile</h4>
    </div>
  <div class="form-group">
    <label for="exampleFormControlInput1">Name</label>
    <input type="text" class="form-control" id="exampleFormControlInput1" placeholder="Name" v-model="editInfo.name">
  </div>
  <div class="form-group">
    <label for="exampleFormControlInput2">Surname</label>
    <input type="text" class="form-control" id="exampleFormControlInput2" placeholder="Surname" v-model="editInfo.surname">
  </div>
  <div class="form-group">
    <label for="exampleFormControlInput2">Email</label>
    <input type="email" class="form-control" id="exampleFormControlInput3" placeholder="Email" v-model="editInfo.email">
  </div>
  <div class="form-group">
    <label for="exampleFormControlInput2">Password</label>
    <input type="password" class="form-control" id="exampleFormControlInput4" placeholder="Password" v-model="editInfo.password">
  </div>
  
   <div class="form-group">
    <button type="button" class="btn btn-secondary btn-lg btn-block" v-on:click.prevent="editCustom">Change password</button>
  </div>

</form>
</div>
    </CWrapper>
</template>

<script>
import TheHeader from '../../containers/TheHeader.vue'
import axios from 'axios'
import forms from '@/mixins/forms';


export default {
  name:"Edit",
  components: {TheHeader,},
  data(){
    return {
       editInfo: {
          name: localStorage.getItem('user-name') || '',
          surname: localStorage.getItem('user-surname') || '',
          email: localStorage.getItem('user-email'),
          password: '',
          token: localStorage.getItem('user-token') || null,
          id: localStorage.getItem('user-id')
        }
    }
  },

  mixins: [ forms ],

  // @todo please use one empty line to separate elements like data, mixins, methods, mounted...
  methods:{
    // @todo my version

      editCustom(){
        const data = { first_name: this.editInfo.name, last_name: this.editInfo.surname, email: this.editInfo.email, password: this.editInfo.password  };
        const token = JSON.parse(JSON.stringify(localStorage.getItem('token')))
        const bearer = 'Bearer ' + token
        console.log(bearer)
        axios({
            method:'put',
            url: `https://api.motivo.localhost/user/${this.editInfo.id}/`,
            data: data,
            headers: { 'Authorization': bearer },
        })
            .then(resp=>{
              console.log(resp)
              window.localStorage.clear();
              this.$router.push('/')
            })
            .catch(error => console.log(error))
            
      }
      
  },
}


 
</script>

<style scoped>
 form {
   width: 100%;
 }

 .formcontainer {
   width: 40%;
   margin: auto;
   margin-top: 15%;
  }
 h4 {
   text-align: center;
   padding-bottom: 1rem;
 }
 a {
   color: #1CB0F6;
   font-weight: bold;
 }

 input, button {
   border-radius: 12px;
   padding: 10px;
 }

 button {
     color: #1CB0F6;
     font-weight: bold;
 }
</style>