<template>
  <div class="c-app">
    <!-- <TheSidebar/> -->
    <CWrapper>
      <TheHeader/>
      <div class="c-body">
        <main class="c-main">
          <CContainer fluid>
            <transition name="fade" mode="out-in">
              <router-view :key="$route.path"></router-view>
            </transition>
          </CContainer>
        </main>
      </div>
      <!-- <TheFooter/> -->
    </CWrapper>
  </div>
</template>

<script>
//import TheSidebar from './TheSidebar'
import TheHeader from './TheHeader'
//import TheFooter from './TheFooter'

export default {
  name: 'TheContainer',
  components: {
    //TheSidebar,
    TheHeader,
    //TheFooter
  },
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
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
