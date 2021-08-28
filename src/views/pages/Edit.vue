<template>
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
</template>

<script>
import axios from 'axios'
import forms from '@/mixins/forms';

export default {
  name:"Edit",

  data(){
    return {
       editInfo: {
          name: '',
          surname: '',
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

      editCustom(){
        const data = { name: this.editInfo.name,surname: this.editInfo.surname, email: this.editInfo.email, password: this.editInfo.password  };
        console.log(data)
        axios
            .put('https://static.motivo.localhost/user/', data)
            .then(resp=>{
              console.log(resp.data)
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