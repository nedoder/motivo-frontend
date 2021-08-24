<template>
<div>
    <div>
       <h4>Sign up</h4>
    </div>
   <form>
       <div class="form-group">
    <label for="exampleFormControlInput1">Name</label>
    <input type="text" class="form-control" id="exampleFormControlInput1" placeholder="Name">
  </div>
  <div class="form-group">
    <label for="exampleFormControlInput2">Surname</label>
    <input type="text" class="form-control" id="exampleFormControlInput2" placeholder="Surname">
  </div>
  <div class="form-group">
    <label for="exampleFormControlInput3">Email</label>
    <input type="email" class="form-control" id="exampleFormControlInput3" placeholder="Email">
  </div>
  <div class="form-group">
    <label for="exampleFormControlInput4">Password</label>
    <input type="password" class="form-control" id="exampleFormControlInput4" placeholder="Password">
  </div>
  <div class="form-group">
    <label for="exampleFormControlInput5">Invite code</label>
    <input type="text" class="form-control" id="exampleFormControlInput5" placeholder="Code">
  </div>
  <div class="form-group">
    <a href="#">Forgot password</a>
  </div>
  <div class="form-group">
    <button type="submit" class="btn btn-info btn-lg btn-block">Sign up</button>
  </div>
  <div class="form-group">
    <button type="button" class="btn btn-secondary btn-lg btn-block">I don't have an account</button>
  </div>
</form>
</div>
</template>

<script>
import CustomSignUp from './CustomSignUp'
export default {
  name: 'CustomSignUp',
  components: { CustomSignUp }
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
 body {
   background-color: #e5e5e5;
 }
 input, button {
   border-radius: 12px;
 }
</style>