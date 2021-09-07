<template>
  <CRow class="d-flex justify-content-center">
    <CCol col="12" lg="6">
       <form @submit.prevent="submitForm">
      <CCard>
        <CCardHeader class='bg-danger text-white'>
          <p>Mandatory</p>
          <h2>Challenge Id: {{ $route.params.id }}</h2>
        </CCardHeader>
        <CCardBody>
          <CRow>
           <div class="field">
            <div class="control">
              <label class="checkbox">
									<input type="checkbox" v-model="form.terms">
									I finished my task. ;)
								</label>
            </div>
          </div>
          </CRow>
          <CRow class='mt-3'>
            <p>Coins to win:<span>{{this.tasks.coins_to_win}}</span></p>
          </CRow>
          <CRow>
            <p>Description:<span>{{this.tasks.description}}</span></p>
          </CRow>
          <CRow>
            <CInputFile label="File input" horizontal invalid-feedback="Please provide a required input." />
          </CRow>
        </CCardBody>
        <CCardFooter class="d-flex justify-content-center">
          <CButton class="col-3" color="danger" size='lg' @click="goBack">Back</CButton>
<button type="submit">Submit</button>        </CCardFooter>
      </CCard>
      </form>
    </CCol>
  </CRow>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Task',
    beforeRouteEnter(to, from, next) {
      next(vm => {
        vm.challengesOpened = from.fullPath.includes('tasks')
      })
    },
    data() {
      return {
        usersOpened: null,
        tasks: null,
        form: {
          terms: false
        }
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
          path: '/dashboard/tasks'
        })
      }
    },
    mounted() {
      const token = localStorage.getItem('user-token')
      const bearer = 'Bearer ' + token
      const tasks = axios({
        method: 'get',

        url: `https://api.motivo.localhost/challenges/${this.$route.params.id}`,

        headers: {
          'Authorization': bearer,
        }
      });

      Promise.all([tasks]).then(([chal]) => {
        console.log(chal);
        this.tasks = chal.data;
        console.log(this.tasks)

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
    submitForm() {
    axios.post('https://api.motivo.localhost/attempt/' ,{
        user: 'blabla',
        challenge: 'bbbb',
        file: null,
        confirmed_by_admin: false
      }).then(response => {
       console.log(response)
      }).catch(error => {
        console.log(error)
      })
    }
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