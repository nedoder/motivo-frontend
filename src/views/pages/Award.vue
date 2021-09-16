<template>
  <CRow class="d-flex justify-content-center">
    <CCol col="12" lg="6">
       <!-- <form @submit.prevent="submitForm"> -->
      <CCard   v-bind:style="{height: '350px', backgroundImage: `url('${this.awards.image}')`, borderRadius:'18px', backgroundSize: 'cover'}">
        <!-- <CCardHeader v-bind:style="{backgroundColor: '#1CB0F6 !important'}">
         
        </CCardHeader>  -->
        <CCardBody class="card-title" v-bind:style="{ color: '#fff', padding: '130px 10px 10px 10px' , margin: '0px', borderRadius: '18px', backgroundColor: 'rgba(53, 57, 53, 0.5)'}">
            <CRow class='m-2'>
             <h2>Title: <span>{{this.awards.title}}</span></h2>
            </CRow>
            <CRow class='m-2'>
          <h6>Description: <span>{{this.awards.description}}</span></h6>
            </CRow>
          <CRow class='m-2' v-bind:style="{color: '#F2C94C', fontWeight: 'bold'}">
             {{this.awards.price_in_coins}} <img src="./img/Coin.png"  class='mx-3' /> 
          </CRow>
          
          <CButton class="col-3 m-2" color="info"  @click="goBack">Back</CButton>
          <CButton class="col-3 m-2"  @click="submitAward">Collect</CButton> 
          <CRow class='m-2'>  
           <p>Message: <span>{{this.message}}</span></p>
          </CRow>
        </CCardBody>
          
                   
      </CCard>
     
    </CCol>
  </CRow>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Award',
    beforeRouteEnter(to, from, next) {
      next(vm => {
        vm.awardsOpened = from.fullPath.includes('awards')
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
          path: '/dashboard/awards'
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
             alert("You collected the award!")
             this.$router.push({
          path: '/dashboard/awards'
        })

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
        this.awards = resp.data;
      }).catch(error => console.log(error))


//POST /api/attempt/
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