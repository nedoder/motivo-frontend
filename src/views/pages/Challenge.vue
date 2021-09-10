<template>
  <CRow class="d-flex justify-content-center">
    <CCol col="12" lg="6">
       <form @submit.prevent="submitForm">
      <CCard v-bind:style="{height: '500px', backgroundImage: `url('${this.tasks.image}')`}">
        <CCardHeader class='bg-info text-white d-flex justify-content-between' >
          <h2 >{{this.tasks.title}}</h2>
          <p class='coin_text'> {{this.tasks.coins_to_win }} &nbsp;   <img class="text-info" src="./img/Coin.png"/></p>
        </CCardHeader>
        <CCardBody>
          <CRow>
            <p>Challenge description:<span> {{this.tasks.description}}</span></p>
          </CRow>
          <CRow>
           <div class="field">
            <div class="control">
              <label >
                Description:
									<input v-model="description">
								</label>
            </div>
          </div>
          </CRow>
          <CRow>
            <CInputFile label="File input" horizontal invalid-feedback="Please provide a required input." />
          </CRow>
        </CCardBody>
        <CCardFooter class="d-flex justify-content-center">
          <CButton class="col-3" color="info" size='lg' @click="goBack">Back</CButton>
<button type="submit">Submit</button>        </CCardFooter>
      </CCard>
      </form>
    </CCol>
  </CRow>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Challenge',
    beforeRouteEnter(to, from, next) {
      next(vm => {
        vm.challengesOpened = from.fullPath.includes('challenges')
      })
    },
    data() {
      return {
        usersOpened: null,
        tasks: null,
        description: ''
      }
    },
    computed: {
      challengeData() {
        const id = this.$route.params.id
        const task = this.challenges.find((task, index) => index + 1 == id)
        const taskDetails = task ? Object.entries(task) : [
          ['id', 'Not found']
        ]
        return taskDetails.map(([key, value]) => {
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
          path: '/dashboard/challenges'
        })
      },

      submitForm() {
        const token = localStorage.getItem('user-token')
        const bearer = 'Bearer ' + token
        const data = {
          user: localStorage.getItem('user-id'),
          challenge: this.$route.params.id,
          description: this.description
        };
        axios({
            method: 'post',
            url: '/api/attempt/',
            data: data,
            headers: {
            'Authorization': bearer,
             }
          }).then(response => {
        alert("You uploaded your solution successfully!")
      this.$router.push('/dashboard/challenges')
       console.log(response)
      }).catch(error => {
        alert("Something went wrong! Please try again.")
        this.$router.push('/dashboard/challenges')
        console.log(error)
      })
    }

    },
    mounted() {
      const token = localStorage.getItem('user-token')
      const bearer = 'Bearer ' + token
      const tasks = axios({
        method: 'get',

        url: `/api/challenges/${this.$route.params.id}`,

        headers: {
          'Authorization': bearer,
        }
      });

      Promise.all([tasks]).then(([chal]) => {
        console.log(chal);
        this.tasks = chal.data;
        console.log(this.tasks)

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
.coin_text {
color: #F2C94C;    
font-weight: bold;
font-size: 24px;
display: flex;
align-items: center;
justify-content: left;
}

pre-content {
  width: 500px;
}
</style>