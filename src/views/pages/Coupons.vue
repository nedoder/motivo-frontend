<template>
  <div>
    <div class="wrapper">
    <CRow fluid  v-bind:style="{border: '1px solid black', borderRadius: '18px !important'}">
      
      <CCol v-for="award in awards" sm="4" v-bind:style="{padding:'1px' , borderRadius: '18px'}">
        <CCard class='bg-secondary' v-bind:style="{backgroundImage: `url('${award.image}')`, height: '300px',color: '#fff'}" @click="couponClicked(award)">
          <!-- <CCardHeader class='bg-info'>
           
          </CCardHeader> -->
          <CCardBody  v-bind:style="{color: '#F2C94C'}">
            <br/>
            {{award.price_in_coins}} <img src="./img/Coin.png" /> 
            <br/>
            <div v-bind:style="{color: '#fff', fontWeight: 'bold'}">
             {{award.title}}
            </div>
          </CCardBody>
          <!-- <CCardFooter class='bg-secondary'>
            Description: {{award.description}} 
          </CCardFooter> -->
        </CCard>
      </CCol>
      <!-- <CCol sm="6">
      </CCol> -->
     
    </CRow>
     </div>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Coupons',
    data() {
      return {
        awards: []
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
      const token = localStorage.getItem('user-token')
      const bearer = 'Bearer ' + token
      console.log(bearer)
      axios({
          method: 'get',
          url: '/api/awards/',
          headers: {
            'Authorization': bearer,
          }
        })
        .then(resp => {
          this.awards = resp.data.results
          // this.title = resp.data.results[0].title
          // this.price = resp.data.results[0].price
          // this.image = resp.data.results[0].image
        })
        .catch(error => console.log(error))
    },

    methods: {

      couponClicked(award) {
                this.$router.push({
                    path: `/dashboard/coupons/${award.id}`
                   
                })
            },
    }
  }
</script>

<style>
  /* .card-columns {
    width: 70%;
    margin: auto;
    margin-top: 130px;
    border-radius: 20px;
  } */

  /* .card-img {
    width: 100%;
    height: auto
  } */

  .wrapper {
    border-radius: 18px;
  }
</style>