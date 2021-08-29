<template>
<div class="wrapper-div">
        <div class="container">
          <img src="./img/motivo.png"/>
        </div>
        <div class="container">
          <CLink to="/dashboard" class="link">Skip</CLink>
        </div>
        <div class="motivo">
          <img src="./img/image(1).png" class="img-fluid" alt="Responsive image"/>
        </div>
        <div class="text">
          <p>Mine points</p>
          <h4>You must complete all mandatory tasks. And optionally recommended with nice to have for extrapoints.  </h4>
        </div>
        <div class="container links">
          <img src="./img/Group 240(1).png"/>
          <CLink to="/onboarding3" class="link two">Next ></CLink>
        </div>
</div>
</template>

<script>
export default {
  name: 'Onboarding2',
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

  .motivo {
    width: 80%;
    margin: 20px auto;
    display: flex;
  }

  .motivo img {
    width: 40%;
    margin: 20px auto;
  }

  .link {
    float: right;
    color: #D3D9DE;
    font-weight: bold;
  }

  .two {
    color: #1CB0F6;
  }

  .links {
    padding: 0;
    margin-top: 40px;
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
