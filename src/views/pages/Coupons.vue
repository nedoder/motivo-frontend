<template>
<Crow>
  <CRow>
    <TheHeader/>
  
  <CRow>
    <div class="card-columns">
  <!-- <div class="card bg-dark text-white">
  <img class="card-img" :src=image alt="Card image">
  <div class="card-img-overlay">
    <h4 class="card-title">{{title}}</h4>
    <p class="card-text">{{price}}</p>
  </div>
</div>
  <div class="card bg-dark text-white">
  <img class="card-img" :src=image alt="Card image">
  <div class="card-img-overlay">
    <h4 class="card-title">{{title}}</h4>
    <p class="card-text">{{price}}</p>
  </div>
</div> -->
  <div class="card bg-dark text-white" v-for="datas in data">
  <img class="card-img" :src=datas.image alt="Card image">
  <div class="card-img-overlay">
    <p class="card-text">{{datas.price_in_coins}}</p>
    <h4 class="card-title">{{datas.title}}</h4>
  </div>
</div>
    </div>
</CRow>
</CRow>
</Crow>
</template>

<script>
import TheHeader from '../../containers/TheHeader.vue'
import axios from 'axios'

export default {
  name: 'Coupons',
  components: {TheHeader,},
  data () {
    return {
      // title: '',
      // price: '',
      // image: '',
      data: []
    }
 
    
  },
//   watch: {
//     $route: {
//       immediate: true,
//       handler (route) {
//         if (route.query && route.query.page) {
//           this.activePage = Number(route.query.page)
//         }
//       }
//     }
//   },
//   methods: {
//     getBadge (status) {
//       switch (status) {
//         case 'Active': return 'success'
//         case 'Inactive': return 'secondary'
//         case 'Pending': return 'warning'
//         case 'Banned': return 'danger'
//         default: 'primary'
//       }
//     },
//     rowClicked (item, index) {
//       this.$router.push({path: `users/${index + 1}`})
//     },
//     pageChange (val) {
//       this.$router.push({ query: { page: val }})
//     }
//   }

mounted() {
    const token = JSON.parse(JSON.stringify(localStorage.getItem('user-token')))
    const bearer = 'Bearer ' + token
    console.log(bearer)
     axios({
       method:'get',
       url: '/api/awards/',
      headers: { 'Authorization': bearer, }
    })
       .then(resp=>{
          console.log(resp.data.results[0])
          this.data = resp.data.results
          // this.title = resp.data.results[0].title
          // this.price = resp.data.results[0].price
          // this.image = resp.data.results[0].image
       })
      .catch(error => console.log(error))
  },
}
</script>

<style>

.card-columns {
  width: 70%;
  margin: auto;
  margin-top: 130px;
  border-radius: 20px;
}

.card-img {
  width: 100%;
  height: auto
}


</style>
