<template>
<div class="wrapper-div">
        <div class="container logo">
          <img src="./img/motivo.png"/>
        </div>
        <div class="container">
          <CLink to="/dashboard" class="link">Skip</CLink>
        </div>
        <div class="motivo">
          <img src="./img/image.png" class="img-fluid" alt="Responsive image"/>
        </div>
        <div class="text">
          <p>Manage tasks easily</p>
          <h4>To change status of task just simply hold & drag it left or right</h4>
        </div>
        <div class="container links">
          <img src="./img/Group 240.png"/>
          <CLink to="/onboarding2" class="link two">Next ></CLink>
        </div>
</div>
</template>

<script>
export default {
  name: 'Onboarding1',

   mounted() {
    const token = JSON.parse(JSON.stringify(localStorage.getItem('user-token')))
    const bearer = 'Bearer ' + token
    console.log(bearer)
     axios({
       method:'get',
       url: 'https://api.motivo.localhost/userdata/',
      headers: { 'Authorization': bearer, }
    })
       .then(resp=>{
          console.log(resp.data.results[0])
          localStorage.setItem('user-id', resp.data.results[0].id)
          localStorage.setItem('user-name', resp.data.results[0].first_name)
          localStorage.setItem('user-surname', resp.data.results[0].last_name)
          localStorage.setItem('user-email', resp.data.results[0].email)
       })
      .catch(error => console.log(error))
  },
}

</script>

<style scoped>
  .wrapper-div {
    height: 100vh;
    background: #ffffff;
  }
  .container {
    padding: 10px;
    margin: auto;
    width: 60%;
  }

  .logo {
    width: 80%;
    padding: 30px;
  }

  .motivo {
    width: 80%;
    margin: auto;
    display: flex;
  }

  .motivo img {
    width: 40%;
    margin: auto;
  }

  .link {
    float: right;
    color: #D3D9DE;
    font-weight: bold;
    text-decoration: none;
  }

  .two {
    color: #1CB0F6;
  }

  .links {
    padding: 0;
    margin-top: 100px;
  }

  .text {
    width: 60%;
    font-size: 18px;
    margin: 20px auto;
    text-align: left;
    color: #99A2AD;
    font-weight: bold;
  }
  h4 {
    font-size: 36px;
    color: #2C2D2E;
  }
</style>