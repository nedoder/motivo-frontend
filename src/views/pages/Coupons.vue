<template>
  <div>
    <CRow fluid>
      <CCol v-for="award in awards" sm="4">
        <CCard class='bg-secondary' v-bind:style="{backgroundImage: `url('${award.image}')`}" @click="couponClicked(award)">
          <CCardHeader class='bg-info'>
            Coupon for: {{award.title}}
          </CCardHeader>
          <CCardBody>
            Price: {{award.price_in_coins}}
          </CCardBody>
          <CCardFooter class='bg-secondary'>
            Description: {{award.description}} 
          </CCardFooter>
        </CCard>
      </CCol>
      <CCol sm="6">
      </CCol>
    </CRow>
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
</style>