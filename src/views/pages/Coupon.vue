<template>
  <CRow class="d-flex justify-content-center">
    <CCol col="12" lg="6">
       <!-- <form @submit.prevent="submitForm"> -->
      <CCard  v-bind:style="{backgroundImage: `url('${this.awards.image}')`}">
        <CCardHeader class='bg-danger text-white'  v-bind:style="{backgroundColor: '#1CB0F6 !important'}">
          <h2>Award id: {{ $route.params.id }}</h2>
          <p>Title:<span>{{this.awards.title}}</span></p>
        </CCardHeader>
        <CCardBody v-bind:style="{height: '500px'}">
          <CRow class='mt-3'>
            <p>Price in coins:<span>{{this.awards.price_in_coins}}</span></p>
          </CRow>
          <CRow>
            <p>Description:<span>{{this.awards.description}}</span></p>
          </CRow>
        </CCardBody>
        <CCardFooter class="d-flex justify-content-center">
          <CButton class="col-3" color="danger" size='lg' @click="goBack" v-bind:style="{backgroundColor: '#1CB0F6 !important'}">Back</CButton>
                <button type="submit" @click="submitAward">Collect the award</button>        </CCardFooter>
      </CCard>
      <p>Message:<span>{{this.message}}</span></p>
    </CCol>
  </CRow>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Coupon',
    beforeRouteEnter(to, from, next) {
      next(vm => {
        vm.awardsOpened = from.fullPath.includes('coupons')
      })
    },
    data() {
      return {
        awardsOpened: null,
        awards: [],
        message: '',
        form: {
          terms: false
        }
      }
    },
    computed: {
      awardsData() {
        const id = this.$route.params.id
        const award = this.awards.find((award, index) => index + 1 == id)
        const awardDetails = award ? Object.entries(award) : [
          ['id', 'Not found']
        ]
        return awardDetails.map(([key, value]) => {
          return {
            key,
            value
          }
        })
      },
    },
    methods: {
      goBack() {
        this.$router.push({
          path: '/dashboard/coupons'
        })
      },

      submitAward() {
          console.log("clicked")
        const token = localStorage.getItem('user-token')
        const bearer = 'Bearer ' + token
        const data = {
          user: localStorage.getItem('user-id'),
          awards: this.$route.params.id,
        };
       axios({
            method: 'post',
            url: 'https://api.motivo.localhost/collectedawards/',
            data: data,
            headers: {
            'Authorization': bearer,
             }
          })
        .then(response => {
             console.log(response)
             this.message = response.data.message
        }).catch(error => {
            console.log(error.response.data.message)
            this.message = error.response.data.message
         })
    },
    },
    mounted() {
      const token = localStorage.getItem('user-token')
      const bearer = 'Bearer ' + token
      const awards = axios({
        method: 'get',

        url: `https://api.motivo.localhost/awards/${this.$route.params.id}`,

        headers: {
          'Authorization': bearer,
        }
      });

      Promise.all([awards]).then(([resp]) => {
        console.log(resp);
        this.awards = resp.data;
        console.log(this.awards)

      }).catch(error => console.log(error))


//POST https://api.motivo.localhost/attempt/
      // {
//       "user": {
//         "username": "nedoder1",
//         "id": 1
//       },
//       "challenge": {
//         "title": "test",
//         "id": 1,
//         "coins": 3,
//         "description": "ddjjd"
//       },
//       "file": null,
//       "confirmed_by_admin": false
//     },
    },
    
  }
</script>

<style scoped>


button {
  color: white;
  border: none;
  width: calc(100% - 30px);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  background: #ccc;
  cursor: pointer;
  box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.3);
  transition: 0.25s all ease;

}

.active {
  background: #5968d7;
}

pre-content {
  width: 500px;
}
</style>