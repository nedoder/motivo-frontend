<template>
<div>
    <div>
       <h4>Sign in</h4>
    </div>
   <form>
  <div class="form-group">
    <label for="exampleFormControlInput1">Email</label>
    <input type="email" class="form-control" id="exampleFormControlInput1" placeholder="Email">
  </div>
  <div class="form-group">
    <label for="exampleFormControlInput2">Password</label>
    <input type="password" class="form-control" id="exampleFormControlInput2" placeholder="Password">
  </div>
  <div class="form-group">
    <a href="#">Forgot password</a>
  </div>
  <div class="form-group">
    <button type="submit" class="btn btn-info btn-lg btn-block">Login</button>
  </div>
  <div class="form-group">
    <button type="button" class="btn btn-secondary btn-lg btn-block">I don't have an account</button>
  </div>
</form>
</div>
</template>

<script>
import CustomLogIn from './CustomLogIn'
export default {
  name: 'CustomLogIn',
  components: { CustomLogIn },
  
}

</script>